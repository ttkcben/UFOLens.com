# GitHub — 第 3 篇（共 3 篇）· 架構筆記（ADR 風格 Discussion）

**用途：** 「Show and tell」／「Architecture」分類下的 Discussion，或 `docs/` ADR 種子。
**關鍵字：** 架構、ADR、forward-only 狀態機、本機 LLM、Ollama、OCR、邊緣運算、CSP、安全標頭、資料管線、成本工程、SQLite manifest、D1、R2、KV
**外部連結：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com 為什麼長這樣

[ufolens.com](https://www.ufolens.com)（[PURSUE UAP 檔案庫](https://www.war.gov/ufo) 的多語可檢索重製版）有三個決定形塑了整個系統，記下來供討論／挑戰。

### 1. 管線刻意做成 forward-only 狀態機

狀態：`discovered → downloaded → ocr_done → translated → published`。文件只前進，且只在有事可做時前進。已發布內容除非 delta 偵測器看到來源真的變了，否則絕不重跑。

**為什麼：** OCR + 翻譯是貴的操作，而檔案庫會隨時間長大。「為了保險全部重跑」的管線成本無上限。讓「往後退」在結構上不可能，「成本暴衝」也跟著不可能。成本上限是狀態圖的性質，不靠運維小心翼翼維持。

**代價：** schema migration 與「刻意重處理」變得很彆扭。可接受的取捨。

### 2. OCR 與翻譯跑在本機 LLM，不用雲端 API

OCR：開源引擎，Tesseract CLI 為後援。翻譯 + NER：Gemma via Ollama，跑在一台 Apple Silicon 筆電。

**為什麼：** 每篇文件邊際成本為零；可重現（固定模型 + 提示詞）；而且擷取那一步本來就得從住宅 IP 跑（來源在 Akamai Bot Manager 後面 —— `curl` 收 403），所以筆電本來就在迴圈裡。

**代價：** 翻譯品質低於前沿模型。對「原文英文永遠一鍵可達的參考語料」而言，這沒問題。我們不主張譯文是權威版本。

### 3. 兩半之間只有一個介面：發布後的 bundle

管線從不直接寫線上資料庫。它輸出 `{ SQL、資產清單、快取清單 }`。「發布」＝ 把這個 bundle 往前套用（SQL 推到邊緣 SQL DB、資產同步到物件儲存、按名清掉指定快取 key）。

**為什麼：** 本機端與邊緣端可以各自演進；bundle 可審核；「部署資料」每次都同一個形狀。Worker 是個小型 TypeScript／Hono 應用 —— 嚴格 CSP（無 `unsafe-inline`；inline JSON-LD 以 sha256 釘選）、`Accept-Language` + 國別→語言協商、30 天 KV 頁面快取、每日清理 cron —— 它完全不需要知道資料是怎麼產出來的。

**代價：** D1 schema 改動會碰兩個檔（`pipeline/lib/manifest_schema.sql`、`db/schema.sql`）。便宜的保險。

### 行為層級的不可動搖原則

- 與美國政府無附屬關係；不使用任何官方標誌。
- 來源遮蔽一律保留，絕不還原。
- 影片歸屬 DVIDS／AARO。
- 全站送 `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` —— 允許搜尋引擎索引，拒絕被當 AI 訓練／爬取資料。

站點：https://www.ufolens.com · API：https://www.ufolens.com/api/v1
