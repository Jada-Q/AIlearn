# Simon Willison《AI state of the union》对我最有用的 10 条

> 2026-05-16 · 按 "能立刻用上 vs 仅哲学层" 排序。

---

## 1. 致命三角（Lethal Trifecta）

私有数据 + 不可信内容 + 外部通信 = 必出 prompt injection。

→ 做 LearnLog / Telegram bridge / agent pipeline 时强制自检三角是否同时存在，存在就拆开。

---

## 2. November 2025 拐点判断

GPT 5.1 / Opus 4.5 起，从"基本能跑"升到"几乎每次都按指令执行"。

→ 影响预设：以前要人工 verify 的环节现在可以更激进让 LLM 自跑（dogfood gate / verify-before-claiming 仍有效，但 LLM 输出端可信度阈值可上调）。

---

## 3. Dark Factory 模式（StrongDM）

没人写也没人读代码，AI swarm 24/7 跑 QA 验证输出。

→ 一人 + AI 公司的极限形态参考。ambient art series + auto-builder 已经在这条路上，可参考 StrongDM 公开案例对照自己的 pipeline。

---

## 4. Red/Green TDD with AI（先 commit 测试再生成实现）

测试先 commit 的关键是防止 AI 改测试迁就实现。

→ CLAUDE.md TDD 决策表已有"先 commit 测试"这一条，Simon 公开验证了这是核心约束，可以加粗强调。

---

## 5. Prototype-First Design（多 variant 并列对比）

不要在单一方案上迭代，让 AI 生成 N 个原型用 usability test 选。

→ 现有 `/prototype` skill 默认单 variant 多轮迭代。改成 N 个并列原型 + 一次选 = 省 3-4 轮 deploy。

---

## 6. Journalist Approach to AI

把 AI 当不可靠信源，记者式交叉验证。

→ 完美契合 CLAUDE.md #1 诚实性规则。可以作为口诀写进：

> **Treat AI output as uncited source — verify ≥2 independent before quoting.**

---

## 7. Pelican Benchmark（画骑自行车的鹈鹕评模型）

自建领域 specific 非正式 eval 持续追踪模型迭代。

→ 我可以做"日本不动产 SaaS 一句话需求 → 落地 prebuild 红队"或"中日双语 portfolio 站 LP 生成"作为自己的 benchmark，每次新模型上线跑一次记录。

---

## 8. Hoarding / Templated Coding（囤积 + 模板模式）

Simon 维护 tools / research repo 积累可复用片段。

→ `~/.claude/skills/` + `~/scripts/` + `~/Desktop/projects-archive/` 就是这个思想的极致实现。Simon 公开验证了路径正确。可以再加一层：把高复用 prompt 提取到 `templates/` 跨项目共用。

---

## 9. Mid-career engineer trap

中等经验工程师最危险（没深度专长可放大 + 没初学者增益）。

→ 我已经在 vertical 护城河（FP / 宅建 / 簿記 / 建筑）+ product / agency 双保险上，但警惕"通用 vibe coder"是中等带。要么继续深耕 vertical knowledge，要么往 service business 走。

---

## 10. Agency 是人类唯一不可替代

AI 永远无法拥有人类动机。

→ 哲学层但有产品定位价值：产品定位口径从"AI 替代 X"改成"AI 放大人的 agency"。
- LearnLog 不是"AI 帮你复习"，是"放大你想学的动机"
- UpgradeMap 不是"AI 帮你选区"，是"放大你判断升值的能动性"

---

## Bonus 候选

- **Wispr Flow**（语音输入工具）— 日常 prompt 输入想提速可以试，Simon 公开背书
- **Claude iPhone app + code.claude.com** — Simon 现在大部分代码在手机上写。移动场景可以试一次（24GB M4 桌前为主，价值有限）

---

## Sources

- [An AI state of the union — YouTube](https://www.youtube.com/watch?v=wc8FBhQtdsA)
- [Lenny's Newsletter article](https://www.lennysnewsletter.com/p/an-ai-state-of-the-union)
- [Simon Willison's blog highlights](https://simonwillison.net/2026/Apr/2/lennys-podcast/)
