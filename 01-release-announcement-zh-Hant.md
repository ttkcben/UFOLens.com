# GitHub — 第 1 篇（共 3 篇）· 發布／README 公告區塊

**用途：** 作為 GitHub Release 內文、釘選的 Discussion，或 repo README 開頭。
**關鍵字：** UAP、UFO、PURSUE 檔案庫、解密文件、開放資料、全文檢索、OCR、機器翻譯、本機 LLM、Ollama、邊緣運算、公開 API、Hono、TypeScript、Python
**外部連結：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — 將 PURSUE UAP 檔案庫多語化、可全文檢索的平台

**站點：** https://www.ufolens.com  ·  **API：** https://www.ufolens.com/api/v1  ·  **原始檔案庫：** https://www.war.gov/ufo

`ufolens.com` 把美國戰爭部公開的 **PURSUE** 解密 UAP／UFO 檔案庫重製成知識平台：跨語料庫的全文檢索、機器翻譯、地圖與時間軸探索，以及一組公開的 JSON API。來源文件為美國聯邦政府職務著作，於美國境內屬公有領域（[17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)）。本專案**與美國政府無附屬關係**，不使用任何官方標誌，也絕不還原原始遮蔽內容。

### 架構

```
本機（Apple Silicon，住宅 IP）                       邊緣網路
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10，核心僅用標準函式庫)         worker/  (TypeScript, Hono.js)
  擷取 → OCR → 翻譯 → 發布（forward-only）             /{lang}/...   頁面
  OCR：開源引擎（Tesseract CLI 為後援）                /api/v1/...   公開 API
  翻譯／NER：本機 LLM（Gemma via Ollama）              /admin        管理後台
  狀態：SQLite manifest                              支援：邊緣 SQL DB、物件儲存
        │                                              （原始 PDF）、KV 快取
        └── 發布 bundle：SQL + 資產清單 + 快取清單 ──┘
```

- **單篇文件雲端 AI 成本為零。** OCR 與翻譯全程跑在本機；forward-only 狀態機（`discovered → downloaded → ocr_done → translated → published`）保證文件未變更就不會重跑。
- **管線核心零第三方依賴** —— 解析／manifest／delta 模組在乾淨的 Python 上、未裝任何 pip 套件即可跑通與通過測試；OCR／翻譯階段在選配套件缺席時優雅降級。
- **邊緣站點**套用嚴格的安全標頭與 CSP（無 `unsafe-inline`；inline JSON-LD 以 sha256 釘選）、以 `Accept-Language` + 國別對映做語言協商、30 天 KV 頁面快取，以及每日清理 cron。
- **增量更新：** delta 偵測器比對來源索引，只把變動部分餵回管線。

### 給開發者

公開 API 於 https://www.ufolens.com/api/v1 以 JSON 回傳文件與後設資料。匿名存取受速率限制；研究者／開發者方案可申請金鑰。完整 endpoints 與限額見站內 API 區。

### 狀態

程式完成；站點已部署於 https://www.ufolens.com。線上資料庫由離線管線跑完後以 `cli_publish run --remote` 推上去灌入。完整設計文件位於 `docs/20260511/`。

### 授權／邊界

- 來源文件：美國聯邦政府職務著作，於美國境內屬公有領域。
- 本平台自身程式碼：見 `LICENSE`。
- 全站送 `Tdm-Reservation: 1` 與 `X-Robots-Tag: noai, noimageai` —— 允許搜尋引擎索引，但拒絕被當 AI 訓練／爬取資料。
- 影片素材歸屬 DVIDS／AARO，本專案不主張任何權利。

歡迎開 issue 與 PR。結構性改動前請先閱讀 `CLAUDE.md` 與 `docs/20260511/00-*`。
