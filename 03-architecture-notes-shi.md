# GitHub — 第 3/3 篇 · 架构笔记 (ADR 风格讨论)

**用途：** 作为 "Show and tell" / "Architecture" 下的讨论，或 `docs/` ADR 种子。
**关键词：** 架构, ADR, 仅向前状态机, 本地 LLM, Ollama, OCR, 边缘计算, CSP, 安全响应头, 数据流水线, 成本工程, SQLite manifest, D1, R2, KV
**超链接：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## 为什么 ufolens.com 采用这种构建方式

关于塑造 [ufolens.com](https://www.ufolens.com)（[PURSUE UAP 存档](https://www.war.gov/ufo) 的可搜索、多语言重建版本）的三项决定的笔记。欢迎评论/反馈。

### 1. 流水线被刻意设计为仅向前状态机 —

状态：`discovered → downloaded → ocr_done → translated → published`。文档仅向前移动，且仅在有工作需要完成时移动。除非 delta 检测器发现源文件确实发生了变化，否则已发布的内容绝不会被重新处理。

**原因：** OCR + 翻译是昂贵的操作，且存档随时间增长。一个“为了安全而重新运行所有内容”的流水线会导致成本无上限。使反向转换成为不可能，从而避免了账单失控。成本上限是状态图的属性，而非取决于操作者的警觉性。

**代价：** 模式迁移和刻意重新处理的操作非常繁琐。这是可以接受的权衡。

### 2. OCR 和翻译运行在本地 LLM 上，而非云端 API

OCR：开源引擎，Tesseract CLI 备选方案。翻译 + NER：通过 Ollama 使用 Gemma，运行在 Apple Silicon 笔记本电脑上。

**原因：** 每份文档的边际成本为零；可复现（固定模型 + 提示词）；且获取步骤已经必须从住宅 IP 运行（源端在 Akamai Bot Manager — 之后，`curl` 会收到 403），因此笔记本电脑本身就在流程中。

**代价：** 翻译质量低于前沿模型。对于一个原英文版本仅需点击一次即可获取的参考语料库来说，这没问题。我们并不声称翻译具有权威性。

### 3. 两部分之间仅共享一个接口：已发布的 bundle

流水线绝不直接写入生产数据库。它生成 `{ SQL, asset manifest, cache-purge list }`。“发布” = 将该 bundle 向前应用（将 SQL 推送到边缘 SQL 数据库，将资产同步到对象存储，清除指定的缓存键）。

**原因：** 本地端和边缘端可以独立演进；bundle 是可审查的；且“部署数据”每次的形状都相同。Worker 是一个小型 TypeScript/Hono 应用 — 严格 CSP（无 `unsafe-inline`；内联 JSON-LD 已 sha256-固定），`Accept-Language` + 国家→语言协商，30 天 KV 页面缓存，每日维护定时任务 —，且它永远不需要知道数据是如何生成的。

**代价：** D1 模式更改会影响两个文件 (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`)。这是廉价的保险。

### 行为中内置的不可协商项

- 不隶属于美国政府；无官方标志。
- 保留源文件的脱敏处理，绝不还原。
- 视频归功于 DVIDS / AARO。
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` 全站 — 可被搜索索引，但拒绝 AI 抓取。

在线地址：https://www.ufolens.com · API：https://www.ufolens.com/api/v1