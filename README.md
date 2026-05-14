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
| 機器核稿級 | 本機 Ollama gemma4:31b | 部分 Tier1+Tier2 主流語（fr、ar、hi、pt-BR、pt-PT、ru…）|
| **主力流程** | **Gemini Pro（手動驅動，prompts 在 `../docs/`）** | **其餘 ~260 語** |

對外發布前所有非 Claude 語言都需要母語審稿者校對。請在 issue / PR 標 `marketing-i18n-review-<lang>`。

## Gemini 工作流（主力）

1. 開啟 `marketing/docs/gemini_prompt_sentences_world_translations.txt`
2. 用 `python3 status.py --prompt-numbers` 看下一個該做的 prompt 編號
3. 在該檔內搜 `PROMPT NNN` 找對應 block，整段 `=== BEGIN PROMPT === … === END PROMPT ===` 貼到 Gemini Pro
4. Gemini 回三份 markdown（中間以 `=== FILE ... ===` 分隔）
5. 把整段回應丟給 `save_gemini_output.py`：

```bash
# stdin 模式（從剪貼簿）
pbpaste | python3 save_gemini_output.py

# 或先存到檔
python3 save_gemini_output.py /tmp/gemini-fr.txt
```

腳本會：偵測語言碼 → 拆三檔 → 結構驗證 → atomic write 到本目錄。

## 工具

| 檔名 | 用途 |
|------|------|
| `status.py` | 顯示完成進度、缺漏的 (語言, 篇) 配對、對應 Gemini PROMPT 編號 |
| `save_gemini_output.py` | 解析 Gemini 多檔輸出並原子寫盤 |
| `translate_all.py` | （備用）本機 Ollama gemma + NLLB-200 雙後端批次譯；用 `--lang xx` 補單一語言 |

```bash
python3 status.py                  # 摘要
python3 status.py --missing        # 一行一個缺漏檔
python3 status.py --prompt-numbers # 缺漏語對應 Gemini PROMPT 編號
python3 status.py --done           # 已完成（3/3）的語言
```

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
