# GitHub — 系列文章 3/3 · 架构笔记（ADR 风格讨论）

**用途：** 作为“展示与分享 (Show and tell)”/“架构 (Architecture)”下的讨论帖，或作为 `docs/` ADR 种子。
**关键词：** 架构, ADR, 仅前向状态机, 本地 LLM, Ollama, OCR, 边缘计算, CSP, 安全响应头, 数据流水线, 成本工程, SQLite 清单, D1, R2, KV
**超链接：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## 为什么 ufolens.com 采用目前的构建方式

关于塑造 [ufolens.com](https://www.ufolens.com)（对 [PURSUE UAP 存档](https://www.war.gov/ufo) 进行的可搜索、多语言重构）的三项决策笔记。欢迎评论/反馈。

### 1. 流水线特意设计为仅前向状态机 —

状态：`discovered → downloaded → ocr_done → translated → published`。文档仅向前推进，且仅在有工作需要处理时才移动。除非 delta 检测器发现源文件确实发生了变化，否则已发布的内容绝不会被重新处理。

**原因：** OCR + 翻译是高成本操作，且存档随时间增长。一个为了“稳妥起见而重新运行所有内容”的流水线会导致成本失控。通过禁止向后转换，可以避免账单爆炸。成本上限是由状态图决定的，而非取决于操作者的警觉性。

**代价：** 模式迁移 (schema migrations) 和特意进行的重新处理操作会比较繁琐。这是可以接受的权衡。

### 2. OCR 和翻译运行在本地 LLM 上，而非云端 API

OCR：开源引擎，Tesseract CLI 作为备选。翻译 + 命名实体识别 (NER)：通过 Ollama 使用 Gemma，运行在 Apple Silicon 笔记本电脑上。

**原因：** 每份文档的边际成本为零；可复现（固定模型 + 提示词）；且抓取步骤必须从住宅 IP 运行（源端在 Akamai Bot Manager — 之后，`curl` 会收到 403 错误），因此无论如何都需要一台笔记本电脑参与。

**代价：** 翻译质量低于顶尖