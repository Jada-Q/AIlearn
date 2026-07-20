# AIlearn

我（Jada）在 vibe coding / AI 协作时代关于职业、心态、技术、方法论的长期学习笔记。

不是 AI 工具教程，也不是 prompt engineering 技巧合集。是一个一人 + AI 公司经营者，在 AI 飞速演化的当下，关于**怎么活、怎么做选择、怎么积累不会贬值的东西**的思考记录。

---

## 索引

### 职业与心态

- [01 — Vibe coding 作为一生职业：心态 / 技术 / 护城河](./docs/01-vibe-coding-as-lifetime-career.md)
- [10 — 和 AI 学技术：一次 1-2 个新概念的渐进式节奏](./docs/10-learning-tech-with-ai-pacing.md)
- [11 — 当人人都会 Vibe Coding：差异化 = 离 AI 平均值的距离，不是奔向它的速度](./docs/11-differentiation-when-everyone-vibe-codes.md)

### 希腊神话 × AI 系列（5 篇完结）

> 五个神话 = 人机协作的五个岗位：约束 / 闭环 / 目标 / 警告 / 外力。每篇：Jada 的实锤 → 神话原型 → 公开科学锚点 → 可迁移检查动作。配双语视频。

- [23 — 奥德修斯的桅杆：规则为什么拦不住你的 AI（约束）](./docs/23-odysseus-mast-forcing-function.md)
- [25 — Echo 与 Narcissus：AI 自食的两种死法（闭环 · model collapse）](./docs/25-echo-narcissus-model-collapse.md)
- [26 — 迈达斯之触：AI 把你的愿望字面执行的那一天（目标 · specification gaming）](./docs/26-midas-touch-specification-gaming.md)
- [27 — 卡珊德拉的诅咒：AI 的警告为什么没人听（警告 · alarm fatigue）](./docs/27-cassandra-warnings-without-hands.md)
- [28 — 皮格马利翁的雕像：为什么"看"救不活你的项目（外力 · ELIZA effect · 收官）](./docs/28-pygmalion-external-force.md)

### 行业判断与方法论

- [02 — Simon Willison《AI state of the union》对我最有用的 10 条](./docs/02-simon-willison-ai-state-of-union.md)
- [03 — 自建 Mac app 过夜消失：从一次故障到 5 层自动化架构](./docs/03-mac-app-overnight-disappear-postmortem.md)
- [04 — Bug 分类决定工具选择：用 TDD 治所有 bug = 用螺丝刀拧所有螺丝](./docs/04-bug-classification-tool-selection.md)
- [06 — 守住判断层：AI 时代如何构建有效验证机制](./docs/06-judgment-layer-verification.md)
- [08 — 撞到物理天花板，就换框架别再调参数：一次实时翻译工具复盘](./docs/08-reframe-at-physical-ceiling.md)
- [18 — 把设计品味变成可测的科学：一场 20 轮 60 评审的盲测实验](./docs/18-design-theory-blind-test.md)
- [19 — 视觉效果选型、质量基准线与判断力培养：一次黑客松连续三次翻车的三层复盘](./docs/19-visual-effects-model-selection-and-judgment-cultivation.md)
- [20 — 省 token 从量自己的账本开始：热文教你装新工具，我搬走了 58KB 旧配置](./docs/20-token-arbitrage-audit-your-own-ledger.md)
- [22 — 设计判断力不是玄学：Norman 框架评审自己的工具 + 素人培养判断力的五个动作](./docs/22-design-judgment-is-learnable.md)
- [29 — 用代码 + AI 生成解说视频的完整管线：录屏为什么糊、Gemini 出图、Veo 动画、ffmpeg 对轴](./docs/29-code-ai-narrated-video-pipeline.md)

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
- [21 — 定时任务里的 AI 助手为什么半夜罢工：换个执行环境，你的脚本就是另一个脚本（launchd 缺 USER / 报错走 stdout / set -e 杀死重试）](./docs/21-launchd-env-silent-cli-failures.md)
- [24 — 「修好了」的幻觉：一个空环境变量骗过两次修复——修复宣告必须含根因一句话](./docs/24-empty-env-var-fixed-twice.md)

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
- [uu-27 — 你向一个工具打听它本身的功能，却忘了自己早已是它的深度用户（资产盲：产出 >> 盘点）](./uu/27-blind-to-your-own-assets.md)
- [uu-28 — 你给自己产品写需求时会漏掉最大卖点：它对你太显然了（知识的诅咒，且只在看到成品时才发现）](./uu/28-curse-of-knowledge-underbrief.md)
- [uu-29 — "我有这个痛"被当成了"我该造它"：痛存在 ≠ 你能靠它赚钱](./uu/29-pain-is-not-monetizable.md)
- [uu-30 — 事实腐烂 vs 时间腐烂：sunset 日期防得住"记录老了"，防不住"记录已经不是真的"](./uu/30-fact-rot-vs-time-rot.md)
- [uu-31 — 视觉惊艳 ≠ 底层结构能力](./uu/31-visual-output-is-not-structural-capability.md)
- [uu-32 — 两套定价系统：给客户按价值定价，给自己按成本定价（定价知识不迁移到自我域）](./uu/32-two-pricing-systems-value-for-clients-cost-for-self.md)
- [uu-33 — 多媒体任务的锚点缺失，与"确认过≠验证过"的确认门陷阱](./uu/33-media-task-anchor-and-confirmation-gate.md)
- [uu-34 — 内部事实修正不会自动传播到公开副本（+ 卖伞人淋雨）](./uu/34-internal-fact-fixes-dont-reach-public-artifacts.md)
- [uu-35 — 生产反射穿了"学习/分享"的戏服：对一篇反过度生产的文章，我连续产出了三次](./uu/35-production-reflex-disguised-as-learning.md)
- [uu-36 — 完美交付冲在商业结构前面：打磨了十几轮，最后才问"这单怎么收钱"](./uu/36-polish-before-commercial-structure.md)
- [uu-37 — 分发是反应式的：即便发，也是想起一个渠道发一个，没计划、没成功信号](./uu/37-distribution-is-reactive-not-systematic.md)
- [uu-38 — 统一的仪表盘诱导你用同一把尺子量所有东西：门面站的"0 数据"不是失败，是量错了](./uu/38-uniform-dashboard-wrong-yardstick.md)
- [uu-39 — 上下文债务不可见：拿着热文找新工具，最大的浪费躺在自己配置里](./uu/39-context-debt-invisible-loading-semantics.md)
- [uu-40 — 判断债：生产指令零延迟，判决请求排队四天——生产是判断的止痛药](./uu/40-judgment-debt.md)
- [uu-41 — 分发者责任盲区：把 AI 生成物交出去的那一刻，你签了一份自己没看见的契约](./uu/41-distribution-liability-blindspot.md)
- [uu-42 — 盲点会跟着你搬进解决方案：一次给监控系统做体检，挖出四个同族盲点](./uu/42-blindspots-copy-into-solutions.md)
- [uu-43 — 校准发生在错误的执行环境 + 教训从不回扫存量](./uu/43-calibrate-in-target-environment.md)
- [uu-44 — 你整理的是文件，喂养混乱的是制度：清理 state，不动 generator](./uu/44-cleanup-state-not-generator.md)
- [uu-45 — 收藏方法 vs 跑轮次：陈述性知识的"已获得"错觉](./uu/45-collecting-methods-vs-running-reps.md)
- [uu-46 — 输出形态是你的旋钮，不是 AI 的默认：透明度错觉让你以为脑中那幅图已经在字里](./uu/46-output-form-is-your-lever.md)
- [uu-47 — 参考只读到"长什么样"，漏掉它同时在演示的"怎么做/能做到什么"：选择性注意让你只抄最亮那层](./uu/47-read-references-at-every-layer.md)
- [uu-48 — 在"给谁看/发去哪/什么算完成"定下来之前，别急着优化交付物：行动偏误让产出感替代了目标定义](./uu/48-define-destination-before-you-optimize.md)
- [uu-49 — 你在批量造"以假乱真的东西"，却没问它做出来给谁、会不会踩线：执行层的爽感盖住了部署层的风险](./uu/49-what-is-this-asset-for.md)
- [uu-50 — 卸载软件时，"删掉"的直觉边界比数据的真实边界小：厂商级共享目录 + 服务器端账户都在删除视野外](./uu/50-uninstall-boundary-blindspot.md)
- [uu-51 — 盲点知识住在判例库里，不住在手上：学盲点的时刻和犯盲点的时刻同日发生而不相认](./uu/51-inert-blindspot-knowledge.md)
- [uu-52 — 一天 8 个改动，用户 0 次使用：批量与反馈周期的错配](./uu/52-batch-size-vs-feedback-cycle.md)
- [uu-53 — 新闻驱动的防御 + 授权了自己读不懂的权力](./uu/53-news-driven-defense.md)
- [uu-54 — 持证 FP 不给自己做家计聆听：问"对我的影响"时没交出自己的暴露面](./uu/54-fp-no-self-intake.md)
- [uu-55 — 应募不是免费期权：搜索词暴露资产盲区 + 决策框架半程被执行冲动截断](./uu/55-applying-is-not-free-optionality.md)
- [uu-56 — 首版验收里没有质量维度：功能过了 ≠ 质量看过了](./uu/56-first-delivery-quality-acceptance.md)
- [uu-57 — 防线防编造，不防自己出题自己判卷：automation bias 三例](./uu/57-automation-bias-self-graded-exams.md)
- [uu-58 — 批准流水线：目的审计发生在交付之后（activity trap + McNamara fallacy）](./uu/58-activity-trap-approval-pipeline.md)
- [uu-59 — 求职渠道在腐烂，巡检清单没覆盖它（transactive memory）](./uu/59-job-channel-rot-inventory-blindness.md)
- [uu-60 — 设计者黑盒感 + 反馈真空期的功能囤积（illusion of explanatory depth + invisible labor）](./uu/60-builder-blackbox-feature-hoarding.md)
- [uu-61 — 「发布」在脑中是按钮，在系统里是没验收过的链路 + 到期类资产没有雷达（last mile problem）](./uu/61-publish-button-and-expiry-blindness.md)
- [uu-62 — 审稿门被连续成功无声侵蚀 + 「造 > 发」同根第 5 次触发升级（automation complacency）](./uu/62-gate-erosion-by-success.md)
- [uu-63 — 当你把一个盲点自动化：从「手动只造不发」升级到「造了一台自动挖兔子洞的机器」（goal displacement / introspection illusion）](./uu/63-automating-the-blindspot.md)
- [uu-64 — "我登录了" ≠ "AI 能操作它"：驱动 AI 前先分清两种"可用"（ELIZA effect / attribute substitution）](./uu/64-logged-in-is-not-operable.md)
- [uu-65 — 学「关于学习的科学」时，正被那门科学要破的错觉骗：建材料库冒充学习（fluency illusion / distribution shift）](./uu/65-building-instead-of-learning.md)
- [uu-66 — 学透一个「解药」后立刻婉拒安装它：理解 fix ≠ 装了 fix（knowing-doing gap / availability heuristic）](./uu/66-knowing-is-not-doing.md)
- [uu-67 — 把「复刻画风」当一根可调旋钮：其实是滤镜 vs 重画两条路，一条有硬天花板（category error / negative transfer）](./uu/67-style-replication-is-a-category-error.md)

---

## 关于这份笔记

**为什么写下来**：AI 工具迭代速度快到让人焦虑。能在 30 年时间尺度上抗通胀的，不是"我会用某个工具"，是"我知道这种问题该怎么想"。这份笔记是后者的积累。

**更新节奏**：不定期。有新洞察才写，不为了更新而更新。

**作者**：[@Jada-Q](https://github.com/Jada-Q) — 在日本，一人 + AI 公司，建筑学出身，跨境不动产 / 财税 / 设计 / AI 交叉位。
