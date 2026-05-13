# GitHub — 第 2 篇（共 3 篇）· 貢獻者招募／good first issues

**用途：** 釘選的 Discussion（「Contributing & good first issues」），或 CONTRIBUTING.md 開頭。
**關鍵字：** 開源、貢獻、good first issue、i18n、在地化、OCR、Python、TypeScript、Vitest、pytest、無障礙、UAP、開放資料
**外部連結：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## 為 ufolens.com 貢獻

[ufolens.com](https://www.ufolens.com) 把美國戰爭部的 [PURSUE UAP 檔案庫](https://www.war.gov/ufo) 變成可全文檢索、多語化的平台，並提供[公開 API](https://www.ufolens.com/api/v1)。專案分兩半 —— 本機 Python 擷取管線（`pipeline/`）與 TypeScript／Hono 邊緣應用（`worker/`）—— 在一個介面相會：發布後的 SQL + 資產 bundle。

貢獻無須任何雲端憑證。管線核心模組只用標準函式庫；Worker 測試在記憶體模擬儲存上跑。

### 環境設置

```bash
# pipeline
python3 -m pytest pipeline/tests/          # 應全綠，零 pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### 最需要幫忙的方向

**i18n／在地化** —— `worker/src/i18n/ui-strings.json` 是 UI 字串來源。任何非英文語系的母語審稿都極有價值：抓機翻怪句、修 RTL／版面、改進語言協商邊界案例。

**OCR 品質** —— 老打字稿掃描在 OCR 前的影像前處理改進；針對樣本頁比較開源引擎 vs. Tesseract 後援的評估腳本。

**無障礙（Accessibility）** —— 對渲染好的頁面（`worker/src/render/`）做 WCAG 稽核；CSP 嚴格（無 `unsafe-inline`），解法必須在這框架內成立。

**API 體驗** —— `worker/src/routes/` —— 分頁、篩選、OpenAPI 描述、範例 client。

**管線韌性** —— 更多優雅降級路徑、更好的進度回報、delta 偵測邊界案例（`pipeline/lib/delta.py`）。

**文件** —— `docs/20260511/`（繁體中文；`00-*` 是索引）。歡迎把設計文件翻譯成英文。

### 守則

- 所有路徑相對 —— 專案必須能跨機器搬移。不接受寫死絕對路徑。
- 不要在 pipeline **核心**模組新增 pip 依賴。選配階段可用選配套件，但在缺套件時必須優雅降級。
- 不要削弱 forward-only 狀態機 —— 那是成本上限。
- 不要引入美國政府官方標誌；不要加任何還原原始遮蔽內容的功能。
- D1 schema 改動會同時動到**兩個**檔：`pipeline/lib/manifest_schema.sql` 與 `db/schema.sql`。
- 新程式要有測試。Commit 訊息走 Conventional Commits。

請先讀 `CLAUDE.md` 與 `docs/20260511/00-*`，結構性改動先開 issue 討論再交 PR。
