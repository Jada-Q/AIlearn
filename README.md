# AIlearn

我（Jada）在 vibe coding / AI 协作时代关于职业、心态、技术、方法论的长期学习笔记。

不是 AI 工具教程，也不是 prompt engineering 技巧合集。是一个一人 + AI 公司经营者，在 AI 飞速演化的当下，关于**怎么活、怎么做选择、怎么积累不会贬值的东西**的思考记录。

---

## 索引

### 职业与心态

- [01 — Vibe coding 作为一生职业：心态 / 技术 / 护城河](./docs/01-vibe-coding-as-lifetime-career.md)
- [10 — 和 AI 学技术：一次 1-2 个新概念的渐进式节奏](./docs/10-learning-tech-with-ai-pacing.md)
- [11 — 当人人都会 Vibe Coding：差异化 = 离 AI 平均值的距离，不是奔向它的速度](./docs/11-differentiation-when-everyone-vibe-codes.md)

### 行业判断与方法论

- [02 — Simon Willison《AI state of the union》对我最有用的 10 条](./docs/02-simon-willison-ai-state-of-union.md)
- [03 — 自建 Mac app 过夜消失：从一次故障到 5 层自动化架构](./docs/03-mac-app-overnight-disappear-postmortem.md)
- [04 — Bug 分类决定工具选择：用 TDD 治所有 bug = 用螺丝刀拧所有螺丝](./docs/04-bug-classification-tool-selection.md)
- [06 — 守住判断层：AI 时代如何构建有效验证机制](./docs/06-judgment-layer-verification.md)
- [08 — 撞到物理天花板，就换框架别再调参数：一次实时翻译工具复盘](./docs/08-reframe-at-physical-ceiling.md)
- [18 — 把设计品味变成可测的科学：一场 20 轮 60 评审的盲测实验](./docs/18-design-theory-blind-test.md)
- [19 — 视觉效果选型、质量基准线与判断力培养：一次黑客松连续三次翻车的三层复盘](./docs/19-visual-effects-model-selection-and-judgment-cultivation.md)

### Vibe Coding 错误

> 翻车后提炼的工作流通则。每条都是具体事故 → 根因分析 → 可固化为下次防护规则。

- [05 — UI 改动方向反掉：3 条 safeguards 修复回路（mockup / canary / channel-aware verify）](./docs/05-ui-change-safeguards.md)
- [07 — 分诊 > 写死规则：有些判断本来就没有标准答案（四版受控实验）](./docs/07-triage-over-stricter-rules.md)
- [09 — 路由 > 切换：带状态的并发资源不要原地切换（打字翻译方向切换失败 3 次的复盘）](./docs/09-route-over-switch.md)
- [12 — 从官方 algorithmic-art skill 到一个生成艺术系列：方法与四个真实 bug（p5 生命周期 / 退化参数 / 工具失配 / bbox 居中）](./docs/12-algorithmic-art-to-series.md)
- [13 — 数据驱动生成艺术 + 把它真正落地：5 个工程与品味判断（数据源 / API 硬约束 / 算法配情感 / 移植非引入 / git cross-check）](./docs/13-data-driven-generative-art-and-shipping.md)
- [15 — 冷热路径验证盲区：「我验证过了」≠「在用户真实路径上验证过」（常开型应用 / 缓存失效 / 让 stale 可见）](./docs/15-cold-vs-warm-path-verification.md)
- [16 — 假绿灯：TDD 防「算错」，smoke test 防「环境不对」（一次 ffmpeg 找不到的复盘）](./docs/16-false-green-tdd-vs-smoke-test.md)
- [17 — AI 说「完成了」时你该问什么：loop 工程的两个陷阱（完成声明≠端到端验证 / 自动化复杂度边际递减）](./docs/17-ai-done-claims-and-loop-complexity.md)

### 数据安全

- [14 — 搬到硬盘 ≠ 备份：一人公司的数据安全系统（3-2-1 / 上云前自加密 / 跨盘搬运避坑）](./docs/14-move-is-not-backup.md)

### Unknown Unknowns（认知盲点）

> 从实战会话里挖"我不知道我不知道"的事，带实锤证据 + 诚实反束（单场观察 ≠ 判决）。

- [uu-01 — 共享环境观察者效应：当你的点击变成 AI 的"灵异 bug"（+ 语音静默纠错层 / 公开复刻署名）](./uu/01-shared-environment-observer-effect.md)
- [uu-02 — 第三方资产引入没有 license 检查这道门：唯一完全静默的缺陷](./uu/02-third-party-asset-license-blindspot.md)
- [uu-03 — 自评分循环的循环性：全绿指标买不来心动](./uu/03-self-scoring-circularity.md)
- [uu-04 — 建好了分发流水线，却没按那个 5 秒的发布键](./uu/04-building-the-pipeline-instead-of-shipping.md)
- [uu-05 — 定制了一个自己已经拥有的工具：库存对决策不可见就等于没有](./uu/05-building-what-you-already-own.md)
- [uu-06 — 验证闸门只守"新建"，"改进/打磨"从缺口溜走（+ AI 快执行抹平了本该让你暂停的摩擦）](./uu/06-improvement-bypasses-the-validation-gate.md)
- [uu-07 — AI 产品的差异化在判断力不在视觉效果（+ 功能难度≠用户感知价值 / 约束先于需求 / 质量基准线）](./uu/07-ai-product-value-is-in-judgment-not-visual-effects.md)
- [uu-08 — 优化目标指令：委托方看见结果，看不见被丢弃的路径](./uu/08-optimization-instruction-black-box.md)
- [uu-09 — 结构型需求不该交给 LLM：生成 vs 后处理的选择盲点](./uu/09-llm-vs-postprocess-routing.md)
- [uu-10 — 工具失败 vs 系统推送：两种心理模型的诊断分岔](./uu/10-tool-failure-mental-model.md)
- [uu-11 — 未收尾就切换 + "多生成几个选项"的隐性成本](./uu/11-unfinished-task-switching.md)
- [uu-12 — "我的文件就在桌面"：home 目录不是桌面，交付物落点的隐形错位（+ 分发包命名与"给谁用"脱钩）](./uu/12-home-folder-is-not-the-desktop.md)
- [uu-13 — 系统状态全盲：你只监控想得到的，想不到的正好咬你](./uu/13-system-state-blindness.md)
- [uu-14 — 全量上書きファイルに「骤減検知」がない：静默データ損失の死角](./uu/14-single-file-metadata-silent-corruption.md)
- [uu-15 — 盲点不是"不知道"，是"知道了也不动"](./uu/15-execution-avoidance-not-ignorance.md)
- [uu-16 — 把"开发工具"当生产后端：CLI 当 runtime 的隐形雷](./uu/16-cli-tool-as-runtime-backend.md)
- [uu-17 — 探索快感盖过需求验证：为"自用工具"越投越深，需求只验证了一下午](./uu/17-exploration-over-need-validation.md)
- [uu-18 — 同一套 Claude，两台机器不是一个大脑：跨机器的治理不会自动跟着走](./uu/18-cross-machine-config-divergence.md)
- [uu-19 — 验证的双重标准：我给 AI 立了铁律，自己执行时却零验证](./uu/19-self-verification-double-standard.md)
- [uu-20 — 元盲点：复盘档案成了"反复观察同一盲点却从不收口"的地方（归档 ≠ 修复）](./uu/20-archive-observes-but-never-escalates.md)
- [uu-21 — 监控显示正常，但"正常"是假的：会报喜的空转 + 叠成墙的门槛](./uu/21-false-green-idle-pipeline.md)
- [uu-22 — "建"伪装成"进步"：专注 ≠ 在一个工具上无限堆功能（全程在建、零 dogfood）](./uu/22-building-disguised-as-progress.md)
- [uu-23 — 用"换工具"回避"定义目标"：没有靶子，任何工具都脱靶（+ 不信自己已做的判断）](./uu/23-tool-shopping-without-a-target.md)
- [uu-24 — 当"学习"变成"生产学习材料"：全程在学却零验证，把 ship 偷换成 build（同根第 4 次）](./uu/24-production-replaces-learning.md)
- [uu-25 — 抽象价值被它最差的载体劫持：舍不得的是原理，绑死在了产品上，于是养出僵尸项目](./uu/25-value-held-hostage-by-its-carrier.md)
- [uu-26 — 你把质量问题归到「自己能碰的最后一步」，天花板却早在上游素材就封死了（手边杠杆偏误）](./uu/26-quality-ceiling-set-upstream.md)

---

## 关于这份笔记

**为什么写下来**：AI 工具迭代速度快到让人焦虑。能在 30 年时间尺度上抗通胀的，不是"我会用某个工具"，是"我知道这种问题该怎么想"。这份笔记是后者的积累。

**更新节奏**：不定期。有新洞察才写，不为了更新而更新。

**作者**：[@Jada-Q](https://github.com/Jada-Q) — 在日本，一人 + AI 公司，建筑学出身，跨境不动产 / 财税 / 设计 / AI 交叉位。
