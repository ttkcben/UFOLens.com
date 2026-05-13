# GitHub — 投稿 1／3 · リリース／README 告知ブロック

**用途：** GitHub Release 本文、ピン留め Discussion、または README 冒頭として。
**キーワード：** UAP、UFO、PURSUE アーカイブ、機密解除文書、オープンデータ、全文検索、OCR、機械翻訳、ローカル LLM、Ollama、エッジコンピューティング、公開 API、Hono、TypeScript、Python
**リンク：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP アーカイブの多言語・全文検索プラットフォーム

**サイト：** https://www.ufolens.com  ·  **API：** https://www.ufolens.com/api/v1  ·  **原典アーカイブ：** https://www.war.gov/ufo

`ufolens.com` は米国戦争省が公開した **PURSUE** 機密解除 UAP／UFO 記録アーカイブを知識プラットフォームとして再公開するプロジェクトです。コーパス全体に対する全文検索、機械翻訳、地図 + タイムラインによる探索、そして公開 JSON API を提供します。原典は米国連邦政府の職務著作物であり、米国内ではパブリックドメインです（[17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)）。本プロジェクトは **米国政府と何ら関連はありません**。公式エンブレムは一切使用せず、原典の塗りつぶしを復元することもありません。

### アーキテクチャ

```
ローカル機（Apple Silicon、家庭用 IP）              エッジネットワーク
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10、コアは標準ライブラリのみ)   worker/  (TypeScript, Hono.js)
  取得 → OCR → 翻訳 → 公開（forward-only）           /{lang}/...   ページ
  OCR：オープンソース・エンジン（Tesseract CLI 補完）/api/v1/...   公開 API
  翻訳／NER：ローカル LLM（Gemma via Ollama）        /admin        運用コンソール
  状態：SQLite manifest                              支える層：エッジ SQL DB、
        │                                              オブジェクトストア（PDF）、KV キャッシュ
        └── 公開 bundle：SQL + 資産マニフェスト + パージリスト ──┘
```

- **1 文書あたりのクラウド AI 費用はゼロ。** OCR と翻訳はすべてローカル実行。forward-only ステートマシン（`discovered → downloaded → ocr_done → translated → published`）が、文書が変わらない限り再処理しないことを保証します。
- **パイプラインのコアにサードパーティ依存ゼロ** —— パース／manifest／delta モジュールは何も `pip install` しなくても素の Python で動き、テストも通ります。OCR／翻訳ステージは任意パッケージが無くてもグレースフルに縮退します。
- **エッジサイト**は厳格なセキュリティヘッダと CSP（`unsafe-inline` 無し、インライン JSON-LD は sha256 で固定）、`Accept-Language` + 国別マッピングによる言語ネゴシエーション、30 日 KV ページキャッシュ、日次ハウスキーピング cron を備えています。
- **増分更新：** delta 検出器が原典インデックスを diff し、変更分だけをパイプラインに戻します。

### 開発者向け

公開 API は https://www.ufolens.com/api/v1 で JSON 形式で文書とメタデータを返します。匿名アクセスはレート制限あり。研究者／開発者ティアはキーを申請してください。エンドポイントと上限はサイトの API セクションを参照。

### ステータス

コード完成。サイトは https://www.ufolens.com で稼働中。本番データベースはオフラインパイプラインを実行し、`cli_publish run --remote` で bundle を投入することで満たします。設計ドキュメントは `docs/20260511/` にあります。

### ライセンス／境界

- 原典：米国連邦政府の職務著作物、米国内ではパブリックドメイン。
- 本プラットフォームのコード：`LICENSE` を参照。
- サイトは `Tdm-Reservation: 1` と `X-Robots-Tag: noai, noimageai` を送出 —— 検索エンジンのインデックスは許可、AI 訓練／スクレイピングからはオプトアウト。
- 映像素材は DVIDS／AARO に帰属し、本プロジェクトは何の権利も主張しません。

Issue・PR 歓迎です。構造的変更の前に `CLAUDE.md` と `docs/20260511/00-*` を必ず読んでください。
