# Bug 分类决定工具选择 — 用 TDD 治所有 bug = 用螺丝刀拧所有螺丝

> 2026-05-17 · 一次 Mac app 故障引出的元方法论：bug 在被修之前先被**分类**

很多人对"工程质量"的默认想象是「写更多测试 / 上 TDD / 多 review」。但 **bug 不是单一类目** — 不同 bug 的根源在不同维度，能拦住它们的工具也完全不同。**用一种工具治所有 bug = 用螺丝刀拧所有螺丝**，多数会失效。

这篇是一个我用得越久越觉得重要的判断框架：**bug 来之前先分类，再挑工具**。

---

## 触发这条经验的具体事故

我自建了一个 Mac 桌面壁纸 app（用 WKWebView 渲染 web 内容到桌面），过夜后桌面壁纸消失。一查 — app 在 display sleep/wake 时崩了（`EXC_BAD_ACCESS` 在 `objc_release` 的 autorelease pool pop 阶段，典型 use-after-free）。

我下意识第一反应是"是不是该写更多测试 / 上 TDD"。

仔细想了下 — TDD 在这里完全不适用：

- ❌ 单元测试里无法 fake macOS 系统 display sleep
- ❌ Crash 在 framework 代码（`objc_release`）里，不在我的代码路径上
- ❌ Race condition 跑 100 次崩 1 次，单元测试可能永远不爆
- ❌ WKWebView 是 Apple 框架对象，生命周期不由我完全掌控

→ 这个 bug 类别根本不在 TDD 的射程内。

但**不代表它没法预防**。该用的工具是另外几个：launchd KeepAlive（resilience layer）+ NSWorkspace sleep/wake handler（defensive coding）+ Address Sanitizer（开发期 use-after-free 检测）+ 操作级 smoke test（pmset displaysleepnow 后看进程还活着）。

→ 抽出来的元方法论：**bug 来之前先问"它属于哪一类"，挑对应类的工具**。

---

## 分类表

| Bug 类型 | 例子 | 最佳工具 |
|---|---|---|
| **逻辑 bug** | 算法、规则、计算、解析、状态机 | **TDD** ✓ |
| **集成 bug** | 多模块/API/数据库/三方服务 交互 | 集成测试 + smoke test |
| **UI bug** | 视觉、布局、交互响应 | 视觉回归（Playwright 截图 diff） |
| **生命周期 / 资源管理 bug** | 内存泄漏、use-after-free、句柄未关 | Sanitizer + 防御 checklist + Resilience layer |
| **系统状态切换 bug** | sleep/wake / login/logout / reboot / 网络切换 / 屏幕变化 | 操作级 smoke test + Resilience layer |
| **性能 bug** | 慢查询、内存膨胀、CPU 飙高 | Profiler + load test |
| **Race condition** | 并发、锁、event 顺序依赖 | Stress test + Thread Sanitizer |

---

## 各类详解

### 1. 逻辑 bug — TDD 真正的射程

判定特征：
- 函数有清楚的 input → output
- 输出确定（同 input 永远同 output）
- 不依赖系统状态 / 时间 / 随机性
- 改一行有可能破其他规则

例子：日期格式化、JSON 解析器、金额计算、URL routing、状态机转换、parser、税务规则、定价逻辑、permission 判定。

TDD 在这里 **真正发挥作用**：写测试 → 写代码让它过 → refactor 不怕破。

**反例**：你做了一个 React 组件用来"显示日期" — TDD 测它吗？大概不值得，UI 视觉问题 TDD 抓不住，逻辑（"今天日期是 5 月 17 日"）也很简单。

### 2. 集成 bug — TDD 的盲区，集成测试的射程

判定特征：
- 涉及多个模块/服务交互
- 单看每个模块都对，组合起来错
- 通常发现于 staging 或 prod，dev 期间正常

例子：A 服务给的字段 B 服务理解错了、第三方 API 改了响应格式、DB 字段类型 migration 后接的旧代码读崩、cache 失效时的 fallback 没走通。

工具：集成测试（真正起 ≥2 个服务跑端到端）+ smoke test（部署后访问关键路径验 200 + 关键字段存在）。

**单元测试覆盖率 100% 但集成 0% 的项目 = 假象的安全**。多见于库（每个函数都测了）但应用没做端到端。

### 3. UI bug — TDD 完全不适用，视觉回归才对

判定特征：
- 写在屏幕上的东西
- 错了肉眼看得出（颜色、位置、字号、对齐）
- 改 CSS 一行有可能破其他页面

例子：button 颜色变了、栅格塌了、移动端 overflow、字体 fallback 没生效、暗黑模式对比度不够。

工具：**视觉回归测试**（Playwright / Percy 截图对比 diff）。基线截图 + 每次 PR 自动 diff，颜色变了或位置错位立刻报。

TDD 测 UI = 测 className 是否含 "btn-primary" 这种琐事，**测不出真正的视觉问题**。

### 4. 生命周期 / 资源管理 bug — Sanitizer + 防御 checklist + Resilience

判定特征：
- 不在你的代码逻辑里，在你怎么使用框架对象的边角
- 表现为崩溃 / 内存泄漏 / 文件句柄耗尽 / 网络连接池满
- 难复现（依赖 GC 时机 / autorelease pool 时机 / OS 状态）

例子：WKWebView delegate 被设置后未 nil → 后续 callback 访问 dangling pointer；NSTimer fire 后 target 已 dealloc；文件 read 后未 close；DB connection 用完未 release。

3 件套：
- **Sanitizer**（dev 期）：Address Sanitizer（AddrSan）抓 use-after-free 在第一时间 crash 并指出对象；Leak Sanitizer 抓泄漏。Swift `-sanitize=address`、Go `-race`、C++ ASan、Python `tracemalloc`。
- **防御 checklist**（写代码时）：每种资源都有一份"创建/使用/释放"的标准流程，强制 review（不能 trust"我记得我 close 了"）。
- **Resilience layer**（prod 期）：launchd KeepAlive / systemd Restart=always / k8s liveness probe。崩了自动重启，**让 bug 不影响用户**。

### 5. 系统状态切换 bug — 操作级 smoke test + Resilience

判定特征：
- 触发于 OS / 环境状态变化的瞬间
- 单纯启动后能正常跑很久
- 必须走过状态切换才暴露

例子：macOS display sleep/wake 后 app 崩；Linux netlink 通知收到后 socket 状态错乱；K8s pod 调度时配置变更；用户切换语言后 cache 没失效；从 Wi-Fi 切到 4G 后 HTTP 请求 hang。

工具：
- **操作级 smoke test**：在 CI 里**真正触发**那个状态切换，然后检查 app 状态。例：
  ```bash
  pmset displaysleepnow && sleep 45 && caffeinate -u -t 1 && \
    pgrep -lf "App" || echo "✗ died during sleep"
  ```
- **Resilience layer**：因为状态切换 bug 经常修不彻底（涉及 OS 内部），先让用户感知不到。

**这一类是 TDD 最弱的方向**，因为状态切换本质上不可单元测试化。

### 6. 性能 bug — Profiler + load test

判定特征：
- 功能对，但慢 / 占内存 / 占 CPU
- 用户在大数据 / 高并发下才感知到

工具：Profiler（Instruments / pprof / Chrome DevTools / py-spy）找瓶颈；load test（k6 / wrk / locust）模拟真实负载。

TDD 测不出性能问题。你可以加性能断言（"这函数 < 100ms"）但跑 100 次 99 次过 1 次超出 = 误报警，没人愿意维护。

### 7. Race condition — Stress test + Thread Sanitizer

判定特征：
- 跑 N 次崩 1 次，跑 100 次崩 5 次
- 加 log 后反而不崩了（log 引入的时延掩盖 race）
- 跟 CPU 数 / 内存压力 / 时序高度相关

工具：Thread Sanitizer（TSan）— 编译期插桩，跑一次就指出 race 的两条线程哪两行代码在抢；stress test 在高并发下反复跑。

TDD 单线程跑测试 = **永远抓不到 race condition**。

---

## 元规则：3 步选择

```
1. 看到 bug → 问"它属于哪一类"
2. 看表查"这一类的最佳工具"
3. 用对应工具 + 设计相应的预防机制（Resilience / Sanitizer / smoke / 回归）
```

**反例 / 信号你用错了工具**：

- 「这个 bug 写更多测试就能预防」→ 多数情况下要么是已经过度 TDD（覆盖率 90%+ 但还出问题），要么是 bug 类型根本不在 TDD 射程内
- 「上 ESLint / TypeScript 就行」→ 静态分析只能抓语法/类型类的，抓不住前 4 类的根本问题
- 「code review 多人看就好」→ 人眼看不出 race / use-after-free / 性能问题

---

## 跟 TDD 决策表的关系（如果你也有一份）

我自己有一份 4 问 + Pre-Gate 的 TDD 决策表（在 CLAUDE.md 项目级）。这次事故里它**正确判定"不该上 TDD"** — Q1 算法/规则？✗。Q2 肉眼看不出？✗ (壁纸消失肉眼立刻看到)。Q3 反复迭代？✓。Q4 之前翻车？✗。得分 1 → smoke test 而非 TDD。

→ **TDD 决策表是对的**。问题不是它判错了，而是 smoke test 在 Mac app 这个 context 里需要包含**操作级**步骤（pmset displaysleepnow），不能只是 build pass。

这次故障让我新加一条到 TDD 决策表：

> **Q5. 是 Mac native / Cocoa 应用？** ✓ → 即便 Q1-Q4 总分进 TDD 区，也优先走 **Address Sanitizer + 操作级 smoke test + Resilience layer**，TDD 是补充不是主力。

---

## 一句话总结

**「写更多测试」 是个懒答案。**

正确的问题是「这个 bug 属于哪一类，哪类最配什么工具」。

逻辑 bug 用 TDD。生命周期 bug 用 Sanitizer。系统状态切换 bug 用操作级 smoke + Resilience。UI bug 用视觉回归。每一类各有最适配的工具。

**用 TDD 治所有 bug = 用螺丝刀拧所有螺丝**。
