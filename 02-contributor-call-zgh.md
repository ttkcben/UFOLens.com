# GitHub — 帖子 2/3 · 徵集貢獻者 / 「適合新手的 issues」

**用途：** 作為置頂的討論區（「貢獻與適合新手的 issues」）或 CONTRIBUTING.md 的簡介。
**關鍵字：** 開源, 貢獻, good first issue, i18n, 本地化, OCR, Python, TypeScript, Vitest, pytest, 無障礙, UAP, 開放數據
**超連結：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## 貢獻於 ufolens.com

[ufolens.com](https://www.ufolens.com) 將美國戰爭部的 [PURSUE UAP 檔案館](https://www.war.gov/ufo) 轉化為一個可搜索的多語言平台，並配備 [公開的 API](https://www.ufolens.com/api/v1)。它由兩個部分 — 本地 Python 導入流水線 (`pipeline/`) 和 TypeScript/Hono 邊緣應用 (`worker/`) — 匯聚於單一接口：一個已發佈的 SQL 與資產包。

您不需要任何雲端憑據即可參與貢獻。流水線的核心模塊僅依賴標準庫 (stdlib-only)，且 Worker 測試在內存存儲中運行。

### 環境搭建

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### 最需要幫助的領域

**i18n / 本地化** — `worker/src/i18n/ui-strings.json` 是 UI 字符串的來源。由母語人士審核任何非英語語言的本地化內容具有極高價值：捕捉生硬的機器翻譯結果、修復 RTL/佈局 issues、優化語言協商 (language-negotiation) 的邊緣情況。

**OCR 質量** — 在 OCR 之前對舊打字機掃描件進行更好的預處理；建立評估框架以比較開源引擎與 Tesseract 回退方案在樣本頁面上的表現。

**無障礙 (Accessibility)** — 根據 WCAG 審核渲染後的頁面 (`worker/src/render/`)；由於 CSP 非常嚴格（不允許 `unsafe-inline`），因此解決方案必須在此限制內實現。

**API 易用性 (Ergonomics)** — `worker/src/routes/` — 分頁、過濾、OpenAPI 描述、示例客戶端。

**流水線魯棒性** — 更多優雅降級的路徑、更好的進度報告、增量檢測 (delta-detection) 的邊緣情況 (`pipeline/lib/delta.py`)。

**文檔** — `docs/20260511/` (繁體中文；`00-*` 為索引)。歡迎將設計文檔翻譯成英文。

### 基本原則

- 所有相對於 — 項目的路徑必須在不同機器間可移植。禁止使用硬編碼的絕對路徑。
- 不要為流水線的 *核心* 模塊添加 pip 依賴。可選階段可以使用可選包，且在缺失這些包時必須能優雅地降級。
- 不要削弱僅前進 (forward-only) 的狀態機 —，這是成本上限。
- 不要引入美國政府的官方標誌，也不要添加任何會撤銷源文件脫敏 (redactions) 的內容。
- D1 schema 的更改涉及 **兩個** 文件：`pipeline/lib/manifest_schema.sql` 和 `db/schema.sql`。
- 新代碼需附帶測試。提交信息請遵循 Conventional-commit 規範。

請先閱讀 `CLAUDE.md` 和 `docs/20260511/00-*`，然後在 PR 之前通過 issue 討論任何結構性的變更。