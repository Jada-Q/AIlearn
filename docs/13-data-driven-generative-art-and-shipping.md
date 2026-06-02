# 数据驱动生成艺术 + 把它真正落地：5 个工程与品味判断

> 2026-06-02 · 接 [doc 12](./12-algorithmic-art-to-series.md)——同一天里把生成艺术从"好看的玩具"推进到"有数据、能送人、上线带演示"的下半场记录

doc 12 讲了照 algorithmic-art skill 做出一个系列。这篇是下半场：把生成艺术接真实数据、接真实产品、真正部署上线时，撞到的 5 个新坑与判断。

---

## 一、数据驱动生成艺术：算法不变，喂真数据，意义就来了

同一套"螺旋 + 刻度"的画法，刻度从随机噪声换成**真实地震数据**（USGS 公开 API），立刻从装饰升级成"有信息的作品"——一年 742 次地震排成生长螺旋，颜色=震源深度，长亮刻度=大震，2011 年那根 M9.1 一眼认出。

**判断**：当一件生成艺术"没意义"时，不一定要换更炫的算法，**换数据源**往往更值钱——尤其当你手里有别人没有的数据/领域。装饰性艺术谁都能做，数据×领域×算法的交叉才是护城河。

## 二、真实 API 有硬约束，先用 count 探明白再设计

想做"有记录以来所有地震"，被现实打回：

- USGS 单次请求**硬上限 2 万条**（实测 `limit=21000` 直接 400）。
- 含小震的全量是**百万级**——全球 M2.5+ 自 1900，连 USGS 自己的 `count` 接口都 503「table full」。

**解法不是硬翻页几百次**，而是**用高震级阈值换可读性**：全球 M7+ 自 1900 = 1609 条，一次拉全、画面也清晰（一圈=一年，跨百年）。

**教训**：接外部 API 做"全量/大范围"功能前，**先用它的 count/HEAD 探真实数量级**，别假设"全部"可加载。约束往往倒逼出更好的产品决策（高阈值反而更美）。

## 三、"不惊艳"的根因常是算法选型不匹配目标，不是参数没调好

做"生日纪念花"，第一版用 Maurer 玫瑰（几何线框）——被否："不惊艳"。

根因不在配色/密度，在**算法本身的气质不匹配情感目标**：线框 = 冷、数学作业感；而纪念品要的是温度。换成**填充花瓣 + 辉光 + 分层深度**（暖色有机花），立刻惊艳。

**教训**：当产物"差口气"时，先问**这个算法的固有气质，和我要的情感/用途匹配吗**。不匹配就换算法，别在错的算法上调参数。"换算法不换参数"在求新意（doc 12）和求情感共鸣（这里）都成立。

## 四、移植而非引入：为贴合既有代码库，把 p5 作品改写成纯 Canvas 2D

要把这朵花接进一个已部署的 vanilla TS 项目（birthday-capsule，刻意无框架、控 bundle）。

**没有**直接引 p5（800KB），而是把花**移植成纯 Canvas 2D**（`bezierCurveTo` + `shadowBlur` + 一个 mulberry32 seeded PRNG，~80 行），只给主 bundle 加了 ~0.3KB。

**教训**：往一个有自己哲学的代码库里加功能，**尊重它的约束**——为单个功能引入重型依赖是污染。移植核心逻辑（算法就几十行）比拖框架进来干净得多。

## 五、git push 撞远程分叉：先 cross-check，别从一个信号下系统结论

push 时被拒（远程有本地没有的提交）。`git diff --name-only main..origin/main` 列出了 `src/bloom.ts` 等我刚改的文件，我一度判断"**远程已经有人做过同名 bloom 集成了**"。

**当场 cross-check 推翻**：`git show origin/main:src/bloom.ts` 返回 **0 行**——远程根本没这文件。真相是 name-only 把"我本地新增"也算进了差异；远程实际只动了 README+docs（另一 session 加的 demo）。用 `git diff base..origin/main` 看清远程真正改了啥，零交集，rebase 干净。

**教训**：这是 CLAUDE.md 规则 #4 的实战——"X 已存在/两个 X 共存"这类系统结论，**必须 ≥2 个独立命令 cross-check** 再下。一个 diff 的方向看反就会得出完全错误的结论并做出破坏性操作（差点 force 覆盖）。

---

## 附：在 GitHub 上展示"动态"——两种 GIF

README markdown 不自动播放 mp4（`<video>` 被过滤），动态只能靠 **GIF**。两种形态：

- **单件动画 GIF**：录一件作品自身的动画（emergence 三件各一条）。
- **拼接 reel / 轮播 GIF**：把多个视图/作品按顺序拼成一条（seismic-rings 切 4 个地区年份；emergence 三件依次播）。

都靠 **headless Chrome 逐帧录 → ffmpeg `palettegen` 两遍法**（先生成调色板再应用，防暗底渐变色带）。区别只是帧来源是"一件的时间序列"还是"多件拼接"。

---

## Sources

- [doc 12 — algorithmic-art 到一个生成艺术系列](./12-algorithmic-art-to-series.md)
- 成品：[seismic-rings](https://github.com/Jada-Q/seismic-rings)（数据驱动）· [emergence](https://github.com/Jada-Q/emergence) · [birthday-capsule](https://github.com/Jada-Q/birthday-capsule)（花已集成）
- 数据源：[USGS Earthquake Catalog (FDSN)](https://earthquake.usgs.gov/fdsnws/event/1/)
