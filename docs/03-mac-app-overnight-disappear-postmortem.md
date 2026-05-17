# 自建 Mac app 过夜消失：从一次故障到 5 层自动化架构

> 2026-05-17 · 一个奇怪 bug 的事后复盘，但学到的东西可以推到任何"多个工作流共享同一系统"的场景

某天早上发现桌面壁纸"消失了" — 回退到 macOS 自带的 Sequoia Sunrise。我自己写的 Mac app（叫 Ambient.app，工作是把一个网页渲染成桌面壁纸）昨晚还好好的，现在不见了。

本文是这次故障的完整复盘 — 病因、5 层治法、5 条可迁移到其他场景的经验。原始 context 是 Mac app 开发，但**核心经验是关于"多个工作流共享同一系统"**，适用于任何并行场景。

---

## 病因（4 步连环）

我用 Claude Code 开了好几个对话窗口同时工作。其中一个窗口里，我建了一个新的 ambient art 作品叫 Moon Phase（这是我的"ambient art series"的第 6 件）。

```
窗口 A: 在做 Moon Phase 新项目
窗口 B: 在维护 ambient-art-pack（Ambient.app 的源）
窗口 C: 在跟我做别的事
```

发生的事：

1. **窗口 B 看到我建了 Moon** → 觉得"该把 Moon 加进 Ambient.app 菜单"
2. **B 改了 Ambient.app 的 Swift 源码** + 重新 build → 生成新版本的 `.app.zip`
3. **但新版本只产了 zip，没装回 `/Applications/Ambient.app`** — 旧 Ambient.app 被关了，新版本卡在工厂没出货
4. **桌面突然没有 app 驱动壁纸了** → macOS 自动回退到默认壁纸

附加隐患是 macOS 的 **App Translocation**（App 流放）机制：

> 如果一个 app 没有合法签名（codesign）+ 又是通过非 Finder 方式（命令行 mv / unzip / postinstall 脚本）放进 `/Applications/` → macOS 怀疑它是病毒，把它放进一个临时只读沙盒（`/private/var/folders/.../AppTranslocation/<UUID>/`）

被流放的 app 跑得不稳，可能凌晨自己就退了。即使窗口 B 装好了新版，没签名也会陷入这个坑。

---

## 病灶诊断

刚开始我猜"是不是 Plash（一个常用的 web→wallpaper Mac 工具）挂了"。
**结果一查 — 我压根没装 Plash。** 一开始的假设全错。

正确的诊断方式：**并行查多个独立假设**，事实出来再动手。

```bash
# 同时跑这几条，看哪个不对
pgrep -lf "Plash"                              # 进程在不在
curl -sS -o /dev/null -w "%{http_code}" $URL   # 网站还通不通
defaults read net.jada.ambient                 # 配置文件被清没
last reboot                                    # 系统昨晚有没有重启
defaults read com.apple.wallpaper              # 当前壁纸是哪个
```

出结果：Plash 没装，URL 正常 200，配置文件**还在但 app 进程没了**，没重启，壁纸已回退默认。
结论：是 Ambient.app 退出了，不是配置丢失或网络问题。

---

## 治法（5 层，越来越根本）

| 层 | 解决了什么 | 比喻 |
|---|---|---|
| **1. 手动装新版本 + ad-hoc 签字** | 当下能用 | 给学生证盖个章，看门人放行 |
| **2. 写脚本 `sign-ambient.sh`** | 把"解压 + 装 + 签 + 重启" 4 步打包成 1 条命令 | 把 4 个独立工具拼成 1 个组合工具 |
| **3. launchd watcher 自动监视 zip mtime** | zip 一变，5 秒后自动跑层 2 — 完全不用动手 | 工厂传送带末端装上自动派送员 |
| **4. 把"作品列表"从 Swift 代码搬到云端 JSON** | 加新作品不需要改 Swift，不需要 rebuild app | 电视频道列表从遥控器外壳搬到云端 |
| **5. 接 GitHub ↔ Vercel auto-deploy** | `git push` 后 Vercel 自动 build 上线 JSON | 流水线接成全自动 |

### 层 1：当下能用 — ad-hoc 签名

```bash
codesign --force --deep --sign - /Applications/Ambient.app
```

`ad-hoc` 签名（用 `-` 作为 identity）不需要 Apple 开发者账号，只是告诉 macOS "这个 app 有自签名，不要 translocate"。完全本地、可逆。

### 层 2：把多步打包成一条命令

```bash
#!/bin/bash
# ~/scripts/sign-ambient.sh — install + sign + relaunch
# Usage: sign-ambient.sh --install <zip> --relaunch
#
# 防止下次 build 出来后 macOS Translocation 把 app 流放进沙盒
```

idempotent + 状态文件防重复触发 + 多种调用入口（--install / --relaunch / 直接 sign）。

### 层 3：launchd watcher

macOS 自带的 "watch 文件变化" 机制：

```xml
<key>WatchPaths</key>
<array>
    <string>/Users/jada/Projects/ambient-art-pack/build/Ambient.app.zip</string>
</array>
<key>ProgramArguments</key>
<array>
    <string>/Users/jada/scripts/ambient-auto-install.sh</string>
</array>
```

任何 session 重 build → zip mtime 变 → launchd 触发我的 wrapper → 5 秒 debounce → 跑层 2 脚本。**我完全不用动手**。

### 层 4：内容下沉云端 — 让 app 二进制几乎不再变

层 1-3 只是把"装新 app"自动化了。层 4 才是治本：**让 app 几乎不需要重 build**。

原来的代码结构（病灶根源）：

```swift
// main.swift — 作品列表硬编码
let PIECES: [Piece] = [
    Piece(key: "tide-pixels", ...),
    Piece(key: "moon-phase", ...),  // ← 加一个 piece 就得改这里
    // ...
]
```

每加一个作品 = 改源码 = 重 build = 出错概率叠加。

重构后：

```swift
// main.swift — 作品列表从云端拉
let REMOTE_URL = URL(string: "https://ambient-art-pack.vercel.app/pieces.json")!

func fetchRemotePieces() {
    URLSession.shared.dataTask(with: REMOTE_URL) { data, _, _ in
        if let data = data,
           let pieces = try? JSONDecoder().decode([Piece].self, from: data) {
            DispatchQueue.main.async { self.pieces = pieces; self.rebuildMenu() }
        }
    }.resume()
}
```

云端就是 GitHub repo 根目录的一个 `pieces.json` 文件，Vercel 自动作为 static file 来 serve。

**fallback 链**（offline / 首次启动也能跑）：

```
1. ~/Library/Caches/net.jada.ambient/pieces.json  ← 上次成功 fetch 的缓存
2. Bundle Resources/pieces.json                    ← ship with .app 的副本
3. 硬编码最小列表（只有 Random rotation）          ← 最后兜底
```

加 "Refresh Pieces from Remote" 菜单 → 用户不重启 app 也能立刻拉最新。

### 层 5：GitHub auto-deploy

```bash
cd ~/Projects/ambient-art-pack && vercel git connect
```

之后 `git push` → Vercel 30-60 秒自动 build + deploy。不需要再手动 `vercel --prod`。

**今后加 piece 的全流程**：

```
edit pieces.json → git push → (完事)
```

不用动 Swift、不用 rebuild、不用碰 `/Applications/`。

---

## 5 条可迁移经验

### ① 内容 vs 代码 — 经常变的东西不该是代码

我之前的错误是把"作品列表"写死在 Swift 源码里 → 加一个 piece = 改代码 = 重 build = 高风险。

**通用规则**：写任何东西时问自己「**这个会经常变吗**」。

- 变 → 数据文件（JSON / CSV / DB / 远程 API）
- 不变 → 代码

**例子**：

- 翻译字符串 → 数据（i18n JSON）
- 业务规则参数（折扣阈值 / 限流值） → 数据（配置文件 / 远程开关）
- UI 文案 → 数据（CMS / 远程文件）
- 业务规则**逻辑**（不变的部分） → 代码

加一行内容就要发版的产品 = 内容/代码边界画错了。

### ② 多个 AI 工作流并行时它们看不见对方

我有 N 个 Claude Code 窗口同时在跑（一个项目一个窗口）。它们：

- 共享同一个 `/Applications/`、`~/.claude/`、`~/.companions/`、剪贴板、本地端口、`/Library/LaunchAgents/`、Anthropic Max 配额池
- **看不见对方的 in-flight 工作**

→ 一个窗口装了新 app，另一个窗口照旧用旧 app；一个窗口在 `:3000` 跑 dev，另一个也想用 `:3000` 撞车；一个窗口 `pkill -f "next dev"` 把另一个窗口的 dev server 一起杀了。

**通用规则**：跨窗口操作走**共享基础设施约定**：

- **Lock 文件**：改全局资源前 acquire lock，避免并发改写
- **固定端口分配**：每个项目自己 `package.json` 里写死端口（写在 `dev` script，不靠记忆）
- **唯一 prefix**：launchd label / 文件名 / API endpoint 都带项目前缀
- **禁用通配杀进程**：`pkill -f "next dev"` 会跨 session 误伤；用 `kill $PID`（具体 PID）

(本系列另有专文讲多窗口 attention budget 分配 — TODO)

### ③ 诊断先于动手 — 别猜，并行查

我一开始想"是 Plash 挂了吧"——你压根没装 Plash。
诊断的正解：**同时跑几个独立 check**，事实出来再动手。

```bash
# bug 出现时的第一步 — 不是"我猜是 X"，而是"我并行验证几个独立维度"
1. 进程层：相关 app 还在跑吗？
2. 网络层：相关 URL 还通吗？
3. 数据层：配置文件 / DB / 缓存被清没？
4. 系统层：OS 重启过吗？系统级配置有没有变？
5. 工具链层：build / deploy / hook 是否正常？
```

**通用规则**：bug 排查 = **同时验证多个独立假设**，不是按猜测顺序一个个试。前者 30 秒得真相，后者 30 分钟还在错的方向上钻。

### ④ 能自动化就别手动

每多一个"你需要记得手动做"的步骤，就多一次"你哪天忘了"的失败概率。

这次堆了 3 层自动化：

```
watcher 监 zip mtime
  → 自动跑 sign 脚本（install + 签字 + 重启）
    → 远程 JSON 让加 piece 不再触发 watcher
      → GitHub auto-deploy 让 push 即上线
```

→ 同样的 bug **不会再发生第 2 次**。

**通用规则**：手动操作出错 1 次 → 立刻问"这步能不能自动化"。

注意：自动化也要做**可观察**（这里是 `/tmp/ambient-watcher.log`） — 失败了能查；和**可关闭**（launchctl bootout） — 出问题能停。

### ⑤ macOS App Translocation 是隐藏陷阱

任何**未签名** + **非 Finder 方式**（命令行 mv / unzip / postinstall）进 `/Applications/` 的 app → 都会被流放到沙盒。

```
/private/var/folders/.../AppTranslocation/<UUID>/d/<App>.app/Contents/MacOS/<App>
```

被流放的 app：

- 不能写自己 bundle 内的文件
- 某些依赖 bundle 路径的功能会坏（Login Items / Preferences）
- macOS 后台清理时可能整个 UUID 目录被回收 → app "凭空消失"

**修法**（脚本化进 build flow，永远别忘）：

```bash
xattr -cr /path/to/App.app                              # 清 quarantine
codesign --force --deep --sign - /path/to/App.app       # ad-hoc 签名
```

之后 macOS 信任这个 app，不再 translocate，跑在 `/Applications/App.app/...` 干净路径。

**更深层规则**：所有"看起来是 macOS 在和你作对"的现象，背后都有具体可定位的安全机制。别归咎"macOS 玄学"，去读那 1 个具体机制的文档，然后写脚本绕开它。

---

## 一句话总结

这次故障表面是「我的 Mac app 过夜消失了」，往下挖是「**多个工作流在同一台机器上共享系统资源时缺乏协调**」，再往下是「**经常变的东西被错放进了代码**」。

修一个 bug 修到第 5 层时，bug 本身已经物理上不可能再发生。这是「**根本治法**」和「**症状治法**」的区别。
