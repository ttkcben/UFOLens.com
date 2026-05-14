# GitHub — 第 2 篇（共 3 篇） · 贡献者招募 / “第一个简单的 issues”

**用途：** 作为置顶讨论（“贡献与第一个简单的 issues”）或 CONTRIBUTING.md 的简介。
**关键词：** 开源, 贡献, good first issue, i18n, 本地化, OCR, Python, TypeScript, Vitest, pytest, 无障碍, UAP, 开放数据
**超链接：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## 为 ufolens.com 做出贡献

[ufolens.com](https://www.ufolens.com) 将美国战争部的 [PURSUE UAP 档案](https://www.war.gov/ufo) 转换为一个可搜索的多语言平台，并配有 [公开的 API](https://www.ufolens.com/api/v1)。它由两部分组成 — 一个本地 Python 摄取流水线 (`pipeline/`) 和一个 TypeScript/Hono 边缘应用 (`worker/`) —，两者通过一个接口对接：一个已发布的 SQL + 资源包。

贡献代码不需要任何云端凭据。流水线的核心模块仅依赖标准库 (stdlib-only)，且 Worker 测试在内存存储中运行。

### 环境搭建

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### 最需要帮助的领域

**i18n / 本地化** — `worker/src/i18n/ui-strings.json` 是 UI 字符串的来源。由母语人士审核任何非英语区域设置具有极高价值：捕捉蹩脚的机器翻译结果，修复 RTL/布局 issues，改进语言协商的边缘情况。

**OCR 质量** — 在 OCR 之前对旧的打字机扫描件进行更好的预处理；建立评估框架，对比开源引擎与样本页上的 Tesseract 回退方案。

**无障碍 (Accessibility)** — 根据 WCAG 审核渲染页面 (`worker/src/render/`)；CSP 要求严格（无 `unsafe-inline`），因此解决方案必须在该限制内实现。

**API 易用性** — `worker/src/routes/` — 分页、过滤、OpenAPI 描述、示例客户端。

**流水线鲁棒性** — 更多优雅的降级路径、更好的进度报告、增量检测的边缘情况 (`pipeline/lib/delta.py`)。

**文档** — `docs/20260511/`（繁體中文；`00-*` 为索引）。欢迎将设计文档翻译成英文。

### 基本原则

- 所有相对于 — 项目的路径必须在不同机器间具有可移植性。不得使用硬编码的绝对路径。
- 不要向流水线 *核心* 模块添加 pip 依赖。可选阶段可以使用可选软件包，且在没有这些软件包时必须能够优雅地降级。
- 不要削弱仅向前的状态机 —，因为那是成本上限。
- 不要引入美国政府的官方标志，也不要添加任何会反转源文件脱敏/遮盖内容的内容。
- D1 Schema 更改涉及 **两个** 文件：`pipeline/lib/manifest_schema.sql` 和 `db/schema.sql`。
- 新代码需附带测试。提交信息需遵循 Conventional-commit 规范。

请先阅读 `CLAUDE.md` 和 `docs/20260511/00-*`，然后在 PR 之前开启一个 issue 以讨论任何结构性问题。