# GitHub — 第 3 篇（共 3 篇）· 架构笔记（ADR 风格 Discussion）

**用途：** 「Show and tell」／「Architecture」分类下的 Discussion，或 `docs/` ADR 种子。
**关键词：** 架构、ADR、forward-only 状态机、本地 LLM、Ollama、OCR、边缘计算、CSP、安全头、数据管线、成本工程、SQLite manifest、D1、R2、KV
**外部链接：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com 为什么长这样

[ufolens.com](https://www.ufolens.com)（[PURSUE UAP 档案库](https://www.war.gov/ufo) 的多语可检索重制版）有三个决定塑造了整个系统，记下来供讨论／挑战。

### 1. 管线刻意做成 forward-only 状态机

状态：`discovered → downloaded → ocr_done → translated → published`。文档只前进，且只在有事可做时前进。已发布内容除非 delta 检测器看到源真的变了，否则绝不重跑。

**为什么：** OCR + 翻译是昂贵的操作，而档案库会随时间增长。「为了保险全部重跑」的管线成本无上限。让「往后退」在结构上不可能，「成本爆冲」也跟着不可能。成本上限是状态图的性质，不靠运维小心翼翼维持。

**代价：** schema 迁移与「刻意重处理」变得很别扭。可接受的取舍。

### 2. OCR 与翻译跑在本地 LLM，不用云端 API

OCR：开源引擎，Tesseract CLI 后备。翻译 + NER：Gemma via Ollama，跑在一台 Apple Silicon 笔电。

**为什么：** 每篇文档边际成本为零；可复现（固定模型 + 提示词）；并且抓取那一步本来就要从住宅 IP 跑（源在 Akamai Bot Manager 后面 —— `curl` 收 403），所以笔电本来就在循环里。

**代价：** 翻译质量低于前沿模型。对「原文英文永远一键可达的参考语料」而言，这没问题。我们不主张译文是权威版本。

### 3. 两半之间只有一个接口：发布后的 bundle

管线从不直接写线上数据库。它输出 `{ SQL、资产清单、缓存清单 }`。「发布」＝ 把这个 bundle 向前应用（SQL 推到边缘 SQL DB、资产同步到对象存储、按名清掉指定缓存 key）。

**为什么：** 本地端与边缘端可以各自演进；bundle 可审计；「部署数据」每次都同一形状。Worker 是个小型 TypeScript／Hono 应用 —— 严格 CSP（无 `unsafe-inline`；inline JSON-LD 以 sha256 钉选）、`Accept-Language` + 国家→语言协商、30 天 KV 页面缓存、每日清理 cron —— 它完全不需要知道数据是怎么产出来的。

**代价：** D1 schema 改动会碰两个文件（`pipeline/lib/manifest_schema.sql`、`db/schema.sql`）。便宜的保险。

### 行为层级的不可动摇原则

- 与美国政府无附属关系；不使用任何官方徽标。
- 源遮蔽一律保留，绝不还原。
- 视频归属 DVIDS／AARO。
- 全站发送 `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` —— 允许搜索引擎索引，拒绝被当作 AI 训练／爬取数据。

站点：https://www.ufolens.com · API：https://www.ufolens.com/api/v1
