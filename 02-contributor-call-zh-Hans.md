# GitHub — 第 2 篇（共 3 篇）· 贡献者招募／good first issues

**用途：** 置顶 Discussion（「Contributing & good first issues」），或 CONTRIBUTING.md 开头。
**关键词：** 开源、贡献、good first issue、i18n、本地化、OCR、Python、TypeScript、Vitest、pytest、无障碍、UAP、开放数据
**外部链接：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## 为 ufolens.com 做贡献

[ufolens.com](https://www.ufolens.com) 把美国战争部的 [PURSUE UAP 档案库](https://www.war.gov/ufo) 变成可全文检索、多语化的平台，并提供[公开 API](https://www.ufolens.com/api/v1)。项目分两半 —— 本地 Python 抓取管线（`pipeline/`）与 TypeScript／Hono 边缘应用（`worker/`）—— 在一个接口相会：发布后的 SQL + 资产 bundle。

贡献无需任何云端凭据。管线核心模块只用标准库；Worker 测试在内存模拟存储上跑。

### 环境配置

```bash
# pipeline
python3 -m pytest pipeline/tests/          # 应全绿，零 pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### 最需要帮忙的方向

**i18n／本地化** —— `worker/src/i18n/ui-strings.json` 是 UI 字符串来源。任何非英文语言的母语审阅都极有价值：抓机翻怪句、修 RTL／版面、改进语言协商边界情况。

**OCR 质量** —— 老打字稿扫描在 OCR 前的图像预处理改进；针对样本页比较开源引擎 vs. Tesseract 后备的评测脚本。

**无障碍（Accessibility）** —— 对渲染好的页面（`worker/src/render/`）做 WCAG 审计；CSP 严格（无 `unsafe-inline`），解法必须在该框架内成立。

**API 体验** —— `worker/src/routes/` —— 分页、筛选、OpenAPI 描述、示例 client。

**管线韧性** —— 更多优雅降级路径、更好的进度汇报、delta 检测的边界情况（`pipeline/lib/delta.py`）。

**文档** —— `docs/20260511/`（繁体中文；`00-*` 是索引）。欢迎把设计文档翻译为英文。

### 规则

- 所有路径相对 —— 项目必须能跨机器迁移。不接受写死绝对路径。
- 不要在 pipeline **核心**模块新增 pip 依赖。可选阶段可用可选包，但在缺包时必须优雅降级。
- 不要削弱 forward-only 状态机 —— 那是成本上限。
- 不要引入美国政府官方徽标；不要加任何还原原始遮蔽内容的功能。
- D1 schema 改动会同时碰**两个**文件：`pipeline/lib/manifest_schema.sql` 与 `db/schema.sql`。
- 新代码要有测试。Commit 消息走 Conventional Commits。

请先读 `CLAUDE.md` 与 `docs/20260511/00-*`，结构性改动先开 issue 讨论再提 PR。
