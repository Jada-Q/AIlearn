# UI 改动方向反掉：3 条 safeguards 修复回路（mockup / canary / channel-aware verify）

> 2026-05-19 · 一次 ambient art 系列 6 项目同步翻车后提炼的工作流通则

Vibe coding 时代，提示精准度只是表层。底下还有几个**结构性回路**——如果设防，单次提示再不准也兜得住；如果不设防，提示再准也会有一次说漏的，整个项目就跟着翻。

这篇是一次具体翻车后提炼的 3 条 safeguards + 1 条架构通则。每一条都独立可救命，3 条同时松才会出我这次的事故。

---

## 触发这条经验的具体事故

我有一个 **ambient art series** 项目——6 个独立的 Next.js 壁纸 piece（潮汐、航班、船舶、地铁、地球地震、月相），每个底部有 6 个圆点 dots 用于切换城市/区域。这 6 个 piece 都通过同一个打包 app（Ambient.app）作为桌面壁纸使用，同时也部署在各自的 vercel URL 上。

我下了一句指令：

> "项目的背景里的 5 个点的地点选择可以去掉吗，不美观，但是**应用版本**的不要去掉。"

意图是：**当壁纸用时隐藏 dots（桌面要纯净）**，**网页打开时保留 dots（朋友点开能切换城市）**。

AI 把"应用版本"理解成了 "Ambient.app 这个 app 里"，做的方向**正好反过来**：
- 网页 vercel URL → 隐藏 dots
- Ambient.app webview 加载 URL → 加 `?embed=app` 显示 dots

12 个文件 edit、6 个 web 项目 commit + push、1 个 ambient-art-pack 改 main.swift + JS endpoints、90 秒等 vercel deploy、playwright 自动截图验证、写完总结报告——整个链跑完后我才发现方向反了。然后反转所有逻辑、6 个项目重新 commit + push、重新 build Ambient.app、再 verify 一遍。

---

## 错的根因不是用词

最容易得出的结论是"中文'应用'有双义（apply vs app）所以用词不当"。这是表层。

底下的真问题是**三个独立的安全网都没设防**：

1. **AI 给我的文字回执（restate）**没拦住歧义。回执里写的是"网页版隐藏 / 壁纸版保留"——但读起来两个方向都合理，我扫一眼"OK"就放过去了。
2. **6 个 sibling 项目同步推**没有 canary。如果先做完一个、我桌面看一下、再推剩下 5 个，第 1 个就抓到方向错了。
3. **验证 surface 不对**。AI 截了浏览器里 vercel URL 的图给我看 "dots 隐藏 ✓"——但 dots 真正使用场景是壁纸时。在浏览器里看 dots 隐藏没问题（因为我以为这是要的方向），但壁纸里的样子根本没人验。直到我自己看桌面才发现。

任何 1 个安全网不松，都能止损在 1 步之内。3 个全松所以走到了"全部做完才发现"。

---

## 3 条 safeguards

### 1. A/B mockup 选方向，替代文字回执

文字回执（"我理解的是 X"）防不住方向歧义。视觉改动**写代码前先出 2 张截图**让用户点哪个。

具体怎么做：

- 改动只有 1 个变量（dots 显示 vs 隐藏）→ puppeteer / playwright 在 dev server 上跑 2 次截屏，一次默认状态、一次切换后状态
- 把 2 张截图并排发给用户，让用户点"我要 A" / "我要 B"
- 点完才动代码

为什么文字回执失效但 mockup 不失效：

- 文字描述同一空间关系的两种方向，句式上都成立（"网页隐藏壁纸保留" vs "网页保留壁纸隐藏"是镜像结构，都符合中文语法）
- 视觉 2 张图的差异是非语言信号，肉眼直接捕获，绕过自然语言歧义层

这其实是 prototype-driven development 的标准做法。但在 vibe coding 里——执行链短、对话密集——很容易省略掉"出 mockup"这一步直接进 code。结果就是文字回执承担它扛不住的责任。

### 2. Canary first：≥3 sibling 项目改同一处时先做 1 个

如果改动要推到 N 个 sibling 项目（series 项目 / 多客户站 / 多 SaaS 模板），**永远先做 1 个、停下来等用户确认 deploy 效果、再推剩下的**。

具体：

- 第 1 个项目改完 → commit + push + deploy → 用户自己打开看 → 说 OK → 才往剩下 N-1 个推
- 第 1 个发现方向错 → 反转 → 重做第 1 个 → 才推剩下
- 这样错误的最大放大倍数是 1×（只第 1 个白做），不是 N×

我这次 6 个项目同时改、同时 push、同时部署——错的方向 → 7 次重做（6 web + 1 ambient-art-pack）。如果走 canary，6× 工作量 → 1× + 5× = 节省 5×。

**关键**：canary 这一步对 AI agent 而言天然反直觉。Agent 倾向于"既然规则一样，一起推完省事"——但这种 batching 在错的方向上是放大器。强制规则比依赖自觉好。

### 3. Channel-aware verify：在用户的真实使用场景验证

UI 改动跨 channel 时（web 浏览器 / 桌面壁纸 / mobile / OG image / 邮件预览 / Slack unfurl），**每个 channel 都要在它自己的环境里验证一次**。验证一个 channel ≠ 验证另一个。

我这次的具体失效：

- AI 在浏览器里打开 vercel URL，screenshot：dots 隐藏 ✓
- 但 dots 真正生效的场景是壁纸——AI 没在 Ambient.app 里加载验证过
- "浏览器里隐藏"看起来 OK，因为这是 AI 以为我要的方向。真正错的方向只有在壁纸里看才暴露

通则：

- web 改动 → 浏览器打开 vercel URL 看 + 模拟 webview 加载看
- 壁纸 / 屏保改动 → 必须在 Ambient.app / Plash / WVS 真实加载看
- mobile 改动 → 真机或正确 viewport（不是浏览器开 devtools 模拟 iPhone）
- 跨 channel 的项目，每个 channel 跑一次 verify 才能 claim 完成

---

## 架构层通则（新项目预防）

跨 channel 不同行为应该用 **route 分离**，不用 **query param 分支**。

这次实现用的是 `?embed=app`——壁纸 app 加这个 param、浏览器默认不加。代码上很优雅（一处判断分两路），但操作上很脆：

- URL 上看不出当前在哪个模式（query param 不显眼）
- vercel CDN cache 可能按 path 分桶但不一定按 param 分桶
- 旧版客户端（已经分发出去的 Ambient.app）不知道新 query param 协议
- playwright session 在不同 query 间会出现意料外的 redirect

更稳的架构：

- `/` 路由 → 显示 dots（web 默认）
- `/embed/` 路由 → 不显示 dots（壁纸 app 加载）

每个路由是物理上分开的两个页面（或者一个 page.tsx + 一个简单 wrapper）。优点：

- URL 直接说明在哪个模式，肉眼可读
- CDN 按 path cache 互不影响
- 新旧客户端只要知道"加载哪个 URL"，不需要协议商讨
- 任何一边可以独立改样式 / 加 feature

代价：稍微多一点代码 boilerplate（重复一些 page.tsx 结构）。但比 "param branching" 的隐性故障率低一个数量级。

**通则**：当一个变量的取值会跨"使用场景"切换时（web vs wallpaper / preview vs full / authenticated vs anon / mobile vs desktop），**优先用 path 区分而不是 query 区分**。query 适合用于"同一场景下的次要变量"（如 city 选择）。

---

## 总结

| Safeguard | 拦住的 failure mode |
|---|---|
| A/B mockup | 方向歧义（文字回执读起来对，做出来反） |
| Canary first | sibling 项目放大效应（错一个变错 N 个） |
| Channel-aware verify | channel mismatch（在 A channel 验证、B channel 用户实际看的是另一回事） |
| Route > query param | 架构脆性（新旧客户端 / cache / 调试可视性） |

任 1 条不松，本次事故都不会发生。**Vibe coding 时代提示精准度是第 0 层，但单靠它扛是反脆性最差的——上面 4 条任何一条都是更结构性的护栏，不依赖单次提示是否说全**。

每次想"这次我说得很清楚了应该不会错"——大概率仍会错。设防的不是"这次的提示"，是"任何一次提示都可能漏"。
