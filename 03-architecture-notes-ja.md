# GitHub — 投稿 3／3 · アーキテクチャ・ノート（ADR 風 Discussion）

**用途：** 「Show and tell」／「Architecture」配下の Discussion、または `docs/` ADR の種。
**キーワード：** アーキテクチャ、ADR、forward-only ステートマシン、ローカル LLM、Ollama、OCR、エッジコンピューティング、CSP、セキュリティヘッダ、データパイプライン、コスト工学、SQLite manifest、D1、R2、KV
**リンク：** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## なぜ ufolens.com はこの形なのか

[ufolens.com](https://www.ufolens.com)（[PURSUE UAP アーカイブ](https://www.war.gov/ufo) の検索可能・多言語な再構築）を形作った 3 つの決断についてのメモ。批判・反論歓迎です。

### 1. パイプラインを意図的に forward-only なステートマシンにしている

状態：`discovered → downloaded → ocr_done → translated → published`。文書は前にしか進まず、しかも仕事があるときにしか進みません。公開済みコンテンツは、delta 検出器が「原典が実際に変わった」と判断した場合を除き、再処理されません。

**理由：** OCR + 翻訳は高コストな操作であり、アーカイブは時間とともに成長します。「念のため全部再実行」するパイプラインはコスト上限がありません。後退を**構造的に**不可能にすることで、暴走した請求書も不可能になります。コスト上限は状態グラフの性質であり、運用者の警戒心に依存しません。

**コスト：** schema マイグレーションや「意図的な再処理」がわざと不便になります。受け入れられるトレードオフです。

### 2. OCR と翻訳はクラウド API ではなくローカル LLM で動かす

OCR：オープンソース・エンジン、Tesseract CLI 補完。翻訳 + NER：Apple Silicon ノートで動く Gemma via Ollama。

**理由：** 1 文書あたりの限界費用ゼロ、再現可能（固定モデル + プロンプト）、そして取得ステップはどのみち家庭用 IP のローカル機から走らせる必要がある（原典は Akamai Bot Manager の後ろ —— `curl` は 403）ので、ノートはどうせループに入っています。

**コスト：** 翻訳品質はフロンティアモデル未満。「英語の原典がワンクリックで見られる参照コーパス」では問題ありません。我々は翻訳が権威版であるとは主張しません。

### 3. 二つの半分の間にあるインターフェースは「公開 bundle」一つだけ

パイプラインは本番データベースを直接書き換えません。`{ SQL、アセット・マニフェスト、キャッシュ・パージリスト }` を出力します。「公開」とは、その bundle を前方向に適用すること（SQL をエッジ SQL DB に push、アセットをオブジェクトストアと同期、指定キャッシュキーをパージ）。

**理由：** ローカル側とエッジ側を独立して進化させられる、bundle はレビュー可能、「データのデプロイ」は毎回同じ形になる。Worker は小さな TypeScript／Hono アプリ —— 厳格 CSP（`unsafe-inline` 無し、インライン JSON-LD は sha256 で固定）、`Accept-Language` + 国 → 言語ネゴシエーション、30 日 KV ページキャッシュ、日次ハウスキーピング cron —— で、データがどう作られたかを知る必要は一切ありません。

**コスト：** D1 schema 変更は二ファイル（`pipeline/lib/manifest_schema.sql`、`db/schema.sql`）に触れます。安い保険。

### 振る舞いに焼き込まれた譲れない原則

- 米国政府と関係なし。公式エンブレム不使用。
- 原典の塗りつぶしは必ず保持、決して復元しない。
- 映像は DVIDS／AARO に帰属。
- サイト全体で `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` —— 検索エンジンインデックス可、AI 訓練／スクレイピングからはオプトアウト。

サイト：https://www.ufolens.com · API：https://www.ufolens.com/api/v1
