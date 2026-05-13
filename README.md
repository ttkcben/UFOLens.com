# GitHub 行銷文 — 多語版本

## 檔案命名

所有譯本皆置於本目錄、單層平面，檔名以 `-{lang}` 結尾，`{lang}` 為 BCP-47 標籤（與 `worker/src/i18n/languages.ts` 的 `ALL_KNOWN_LANGS` 對齊）：

```
01-release-announcement-en.md          ← 英文 canonical（權威來源，要改先改這裡）
01-release-announcement-zh-Hant.md     ← 繁體中文
01-release-announcement-zh-Hans.md     ← 简体中文
01-release-announcement-ja.md          ← 日本語
01-release-announcement-pt-BR.md       ← Português (Brasil)
01-release-announcement-zh-CN.md       ← 在 fallback chain 內視為 zh-Hans 別名（不另產生檔）
...
02-contributor-call-<lang>.md
03-architecture-notes-<lang>.md
```

帶 Script 子標籤（`zh-Hant` / `pt-BR` / `ks-Deva`）的檔名直接保留破折號；ASCII 與大小寫須與 BCP-47 完全一致。

## 覆蓋範圍

涵蓋網站支援的 272 種語言（`ALL_KNOWN_LANGS` 全集）。實際發布時以 D1 的 `ufo_site_meta('languages_published')` 決定哪些上線；其他保留作為後備與 hreflang 候選。

## 翻譯品質分層

| 層級 | 引擎 | 語言 |
|------|------|------|
| 人工核稿級 | Claude（Anthropic Opus） | en（canonical）、zh-Hant、zh-Hans、ja、es、de |
| 機器翻譯級 | 本機 Ollama gemma4:31b | 其餘 ~266 語 |

機器翻譯層需要母語審稿者校對才能對外發布。請在 issue / PR 標 `marketing-i18n-review-<lang>`。

## 重生指令

英文原稿改動後，要重跑機器翻譯部分：

```bash
cd marketing/20260513/github
# 預熱本機 Ollama（消除 ~17s 冷啟動）
python3 translate_all.py --warmup
# 批次翻譯（冪等：已存在的檔案不會覆蓋）
python3 translate_all.py --all
# 指定單一語言重翻
python3 translate_all.py --lang fr
# 強制重翻所有語言（覆蓋既有）
python3 translate_all.py --all --force
```

執行日誌寫到 `./translate_all.log`。預估全 266 語 × 3 篇 ≈ 30–40 小時（gemma4:31b 在 M1 Max 約 14 tok/s）；建議週末跑或 launchd 排程。

## 法律邊界（每個語言版本必須保留下列敘述）

- 與美國政府無附屬關係（not affiliated）；不使用任何官方標誌
- 來源檔案於美國境內為公有領域（17 U.S.C. §105）；本站程式碼授權見 `LICENSE`
- 影片歸屬 DVIDS / AARO，本專案不主張任何權利
- 全站送 `Tdm-Reservation: 1` 與 `X-Robots-Tag: noai, noimageai`

如譯本對上述敘述有任何稀釋或誤譯，請優先回報 issue（高優先處理）。

## 翻譯凍結欄位

下列字串在所有譯本中**不翻譯**（程式識別字 / URL / 法律術語）：

- URL（`https://...`、`www.ufolens.com`、`war.gov` 等）
- 程式碼區塊（` ``` ` 內）
- 反引號內聯碼（`` `cli_publish` ``、`` `pipeline/lib/` `` 等）
- 法源編號（`17 U.S.C. §105`）
- 標頭名（`Tdm-Reservation`、`X-Robots-Tag`、`noai`、`noimageai`）
- 機構縮寫（`DVIDS`、`AARO`、`PURSUE`、`UAP`、`UFO`、`OCR`、`CSP`、`API`、`JSON`、`JSON-LD`、`OG`、`KV`、`R2`、`D1`、`SQL`、`SQLite`、`NER`、`MIT`、`OpenAPI`、`WCAG`）
- 套件 / 工具名（`Hono.js`、`Hono`、`TypeScript`、`Python`、`Vitest`、`pytest`、`Wrangler`、`Cloudflare`、`Tesseract`、`Ollama`、`Gemma`、`Akamai Bot Manager`、`Apple Silicon`、`NLLB`）
- 商標 / 域名（`ufolens.com`、`@sonicjs-cms/core`）
