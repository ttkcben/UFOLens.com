# GitHub — 第 1 篇（共 3 篇）· 发布／README 公告区块

**用途：** 作为 GitHub Release 正文、置顶 Discussion，或仓库 README 顶部。
**关键词：** UAP、UFO、PURSUE 档案库、解密文件、开放数据、全文检索、OCR、机器翻译、本地 LLM、Ollama、边缘计算、公开 API、Hono、TypeScript、Python
**外部链接：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — 把 PURSUE UAP 档案库多语化、可全文检索的平台

**站点：** https://www.ufolens.com  ·  **API：** https://www.ufolens.com/api/v1  ·  **原始档案库：** https://www.war.gov/ufo

`ufolens.com` 把美国战争部公开的 **PURSUE** 解密 UAP／UFO 档案库重制为知识平台：跨语料库的全文检索、机器翻译、地图与时间轴探索，以及一组公开的 JSON API。源文件为美国联邦政府职务作品，在美国境内属公有领域（[17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)）。本项目**与美国政府无附属关系**，不使用任何官方徽标，也绝不还原原始遮蔽内容。

### 架构

```
本地（Apple Silicon，住宅 IP）                       边缘网络
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10，核心仅用标准库)             worker/  (TypeScript, Hono.js)
  抓取 → OCR → 翻译 → 发布（forward-only）            /{lang}/...   页面
  OCR：开源引擎（Tesseract CLI 后备）                 /api/v1/...   公开 API
  翻译／NER：本地 LLM（Gemma via Ollama）             /admin        管理后台
  状态：SQLite manifest                              支撑：边缘 SQL DB、对象存储
        │                                              （原始 PDF）、KV 缓存
        └── 发布 bundle：SQL + 资产清单 + 缓存清单 ──┘
```

- **单篇文档云端 AI 成本为零。** OCR 与翻译全程跑在本地；forward-only 状态机（`discovered → downloaded → ocr_done → translated → published`）保证文档未变更不会重跑。
- **管线核心零第三方依赖** —— 解析／manifest／delta 模块在干净 Python、未装任何 pip 包时即可跑通与通过测试；OCR／翻译阶段在可选包缺席时优雅降级。
- **边缘站点**应用严格的安全头与 CSP（无 `unsafe-inline`；inline JSON-LD 以 sha256 钉选）、以 `Accept-Language` + 国家映射做语言协商、30 天 KV 页面缓存，以及每日清理 cron。
- **增量更新：** delta 检测器比对源索引，仅把变更部分喂回管线。

### 给开发者

公开 API 位于 https://www.ufolens.com/api/v1 ，以 JSON 返回文档与元数据。匿名访问受速率限制；研究者／开发者方案可申请密钥。完整 endpoints 与限额见站内 API 区。

### 状态

代码完成；站点已部署于 https://www.ufolens.com。线上数据库由离线管线跑完后以 `cli_publish run --remote` 推上去灌入。完整设计文档位于 `docs/20260511/`。

### 授权／边界

- 源文件：美国联邦政府职务作品，在美国境内属公有领域。
- 本平台自身代码：见 `LICENSE`。
- 全站发送 `Tdm-Reservation: 1` 与 `X-Robots-Tag: noai, noimageai` —— 允许搜索引擎索引，但拒绝被当作 AI 训练／爬取数据。
- 视频素材归属 DVIDS／AARO，本项目不主张任何权利。

欢迎提 issue 与 PR。结构性改动前请先阅读 `CLAUDE.md` 与 `docs/20260511/00-*`。
