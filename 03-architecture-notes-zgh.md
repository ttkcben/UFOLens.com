# GitHub — 第 3/3 篇 · 架构笔记 (ADR 风格讨论)

**用途：** 在 "Show and tell" / "Architecture" 下的讨论，或作为 `docs/` ADR 种子。
**关键词：** 架构, ADR, 仅前向状态机, 本地 LLM, Ollama, OCR, 边缘计算, CSP, 安全响应头, 数据流水线, 成本工程, SQLite 清单, D1, R2, KV
**超链接：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## 为什么 ufolens.com 要这样构建

关于塑造 [ufolens.com](https://www.ufolens.com) (对 [PURSUE UAP 存档](https://www.war.gov/ufo) 进行的可搜索、多语言重构) 的三个决策的笔记。欢迎评论/反馈。

### 1. 流水线被刻意设计为仅前向状态机 —

状态：`discovered → downloaded → ocr_done → translated → published`。文档仅向前推进，且仅在有工作需要处理时才推进。除非增量检测器发现源文件确实发生了变化，否则已发布的内容绝不会被重新处理。

**原因：** OCR + 翻译是昂贵的操作，且存档随时间增长。一个“为了保险起见重新运行所有内容”的流水线会导致成本无上限。通过禁止向后转换，可以避免账单失控。成本上限是状态图的属性，而非取决于操作者的警觉性。

**代价：** 模式迁移和刻意重新处理的操作被设计得较为繁琐。这是可接受的权衡。

### 2. OCR 和翻译运行在本地 LLM 上，而非云端 API

OCR：开源引擎，Tesseract CLI 备选方案。翻译 + NER：通过 Ollama 使用 Gemma，运行在 Apple Silicon 笔记本电脑上。

**原因：** 每份文档的边际成本为零；可复现 (固定模型 + 提示词)；且抓取步骤已经必须从住宅 IP 运行 (源端位于 Akamai Bot Manager — 之后，`curl` 会收到 403 错误)，因此无论如何都需要一台笔记本电脑参与。

**代价：** 翻译质量低于前沿模型。对于一个英文原文仅需点击一次即可查看的参考语料库来说，这没有问题。我们不声称翻译具有权威性。

### 3. 两部分之间仅共享一个接口：已发布的 bundle

流水线绝不直接写入生产数据库。它输出 `{ SQL, asset manifest, cache-purge list }`。“发布” = 将该 bundle 向前应用 (将 SQL 推送到边缘 SQL 数据库，将资源同步到对象存储，清除指定的缓存键)。

**原因：** 本地端和边缘端可以独立演进；bundle 是可审查的；且“部署数据”的格式每次都相同。Worker 是一个小型 TypeScript/Hono 应用 — 严格的 CSP (无 `unsafe-inline`；内联 JSON-LD 已固定至 sha256)，包含 `Accept-Language` + 国家 $\rightarrow$ 语言协商、30 天 KV 页面缓存、每日维护的 cron —，且它无需关心数据是如何生成的。

**代价：** D1 模式更改涉及两个文件 (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`)。这是低成本的保险。

### 内置于行为中的不可妥协项

- 不隶属于美国政府；无官方标志。
- 保留源文件的脱敏/遮盖内容，绝不还原。
- 视频归属 DVIDS / AARO。
- 全站 `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` — 可被搜索索引，但禁止 AI 抓取。

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1