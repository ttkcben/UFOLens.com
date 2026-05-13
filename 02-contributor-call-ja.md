# GitHub — 投稿 2／3 · コントリビュータ募集／good first issues

**用途：** ピン留め Discussion（「Contributing & good first issues」）、または CONTRIBUTING.md 冒頭として。
**キーワード：** オープンソース、貢献、good first issue、i18n、ローカライズ、OCR、Python、TypeScript、Vitest、pytest、アクセシビリティ、UAP、オープンデータ
**リンク：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com へのコントリビュート

[ufolens.com](https://www.ufolens.com) は米国戦争省の [PURSUE UAP アーカイブ](https://www.war.gov/ufo) を、検索可能・多言語のプラットフォームと[公開 API](https://www.ufolens.com/api/v1) に変換するプロジェクトです。構造は二つの半分 —— ローカル Python の取得パイプライン（`pipeline/`）と TypeScript／Hono のエッジアプリ（`worker/`）—— が、公開済み SQL + アセット bundle という単一インターフェースで出会う形になっています。

貢献にクラウドの認証情報は不要です。パイプラインのコアモジュールは標準ライブラリのみ、Worker のテストはインメモリストレージで動きます。

### セットアップ

```bash
# pipeline
python3 -m pytest pipeline/tests/          # オールグリーンになるはず（pip install 不要）

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### 特に助かる領域

**i18n／ローカライズ** —— `worker/src/i18n/ui-strings.json` が UI 文字列のソースです。非英語ロケールのネイティブレビューは非常に価値が高い：機械訳のぎこちなさを潰す、RTL／レイアウト問題を直す、言語ネゴシエーションのエッジケースを改善する、など。

**OCR 品質** —— 古いタイプライタ原稿のスキャンを OCR にかける前の前処理改善。サンプルページでオープンソース・エンジンと Tesseract 補完を比較する評価ハーネス。

**アクセシビリティ** —— レンダリング済みページ（`worker/src/render/`）の WCAG 監査。CSP が厳格（`unsafe-inline` 無し）なので、解決策はその制約内で成立する必要があります。

**API の使い心地** —— `worker/src/routes/` —— ページング、フィルタリング、OpenAPI 記述、クライアントサンプル。

**パイプラインの堅牢性** —— グレースフルな縮退経路を増やす、進捗レポートを改善する、delta 検出のエッジケース（`pipeline/lib/delta.py`）。

**ドキュメント** —— `docs/20260511/`（繁体中国語；`00-*` が目次）。設計ドキュメントの英訳も歓迎です。

### 守るべきルール

- パスはすべて相対 —— プロジェクトはマシン間でポータブルである必要があります。絶対パスのハードコードはダメ。
- パイプラインの**コア**モジュールに pip 依存を追加しないこと。任意ステージは任意パッケージを使ってよいが、無くてもグレースフルに縮退すること。
- forward-only ステートマシンを弱めないこと —— それがコスト上限です。
- 米国政府の公式エンブレムを持ち込まない。原典の塗りつぶしを復元する処理を入れない。
- D1 schema 変更は**二つ**のファイルに触れます：`pipeline/lib/manifest_schema.sql` と `db/schema.sql`。
- 新コードにはテストを。コミットメッセージは Conventional Commits 準拠。

`CLAUDE.md` と `docs/20260511/00-*` を先に読み、構造的変更は PR の前に issue で議論してください。
