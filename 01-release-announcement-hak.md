# GitHub — 第 1 篇 / 3 · 發布 / README 公告區塊

**用途：** GitHub 發布內容、置頂討論或儲存庫 README 頂部。
**關鍵字：** UAP, UFO, PURSUE 檔案, 解密文件, 開放資料, 全文搜尋, OCR, 機器翻譯, 本地 LLM, Ollama, 邊緣運算, 公開 API, Hono, TypeScript, Python
**超連結：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — 一個多語言、可搜尋的 PURSUE UAP 檔案平台

**上線：** https://www.ufolens.com · **API：** https://www.ufolens.com/api/v1 · **原始檔案：** https://www.war.gov/ufo

`ufolens.com` 將美國戰爭部解密嘅 UAP / UFO 記錄嘅 **PURSUE** 檔案重新發布為知識平台：提供全文搜尋、語料庫機器翻譯、地圖 + 時間軸探索，以及公開 JSON API。原始文件係美國聯邦政府嘅作品，在美國境內屬於公共領域 ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105))。本項目**並非美國政府嘅附屬機構**，不使用任何官方標誌，也絕不逆轉密文。

### 架構

```
Local machine (Apple Silicon, residential IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **每個文件嘅雲端 AI 成本為零。** OCR 同翻譯都在本地執行；向前推進嘅狀態機 (`discovered → downloaded → ocr_done → translated → published`) 確保除非文件有更改，否則不會重新處理。
- **Pipeline 核心無第三方依賴** — 解析 / 清單 / 增量模組在無安裝任何 pip 包嘅純 Python 環境下運行同測試；OCR/翻譯階段在缺少可選包時會優雅降級。
- **邊緣站點** 應用嚴格嘅安全標頭 + CSP (無 `unsafe-inline`；內聯 JSON-LD sha256 釘選)，通過 `Accept-Language` + 國家映射進行語言協商，一個 30 天嘅 KV 頁面快取，以及每日內務管理定時任務。
- **增量更新：** 一個增量檢測器會比較源索引，並只將更改回饋到管道中。

### 針對開發者

公開 API https://www.ufolens.com/api/v1 返回 JSON 格式嘅文件同元數據。匿名訪問受速率限制；研究人員/開發者層級請申請金鑰。請參閱網站上嘅 API 部分以了解端點和限制。

### 狀態

代碼完成；網站部署在 https://www.ufolens.com。生產數據庫通過運行離線管道並向前發布捆綁包 (`cli_publish run --remote`) 填充。完整設計文檔存放在 `docs/20260511/` 中。

### 許可 / 邊界

- 原始文件：美國聯邦政府作品，在美國境內屬公共領域。
- 本平台嘅代碼：請參閱 `LICENSE`。
- 網站發送 `Tdm-Reservation: 1` 和 `X-Robots-Tag: noai, noimageai` — 可被搜尋引擎索引，但已選擇退出 AI 訓練/抓取。
- 影像片段歸因於 DVIDS / AARO，本項目不主張其所有權。

歡迎提出問題同 PRs。在提出結構性更改之前，請閱讀 `CLAUDE.md` 和 `docs/20260511/00-*`。

