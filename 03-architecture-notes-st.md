# GitHub — Khasiho 3 ho 3 · Lintlha tsa Moralo oa Mohaho (Puisano ea mokhoa oa ADR)

**Sebelisa e le:** Puisano tlas'a "Show and tell" / "Architecture", kapa ADR seed ea `docs/`.
**Mantsoe a bohlokoa:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Lihokelo:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Lebaka leo ufolens.com e hahiloeng ka lona ka tsela eo e leng ka eona

Lintlha mabapi le liqeto tse tharo tse thehileng [ufolens.com](https://www.ufolens.com) (ho hahoa bocha bo ka batlisisoang, ba lipuo tse ngata ba [polokelo ea PURSUE UAP](https://www.war.gov/ufo)). Litlhaloso / khanyetso lia amoheleha.

### 1. Pipeline ke mochini oa boemo o tsoelang pele feela — ka boomo

Maemo: `discovered → downloaded → ocr_done → translated → published`. Tokomane e ea pele feela, 'me feela ha ho na mosebetsi o lokelang ho etsoa. Litaba tse phatlalalitsoeng ha li sebetsoe hape ntle le haeba mochine o fumanoang oa delta o bona hore mohloli o fetohile.

**Lebaka:** OCR + phetolelo ke lits'ebetso tse theko e boima, 'me polokelo e hola ka nako. Pipeline e "tsamaisang ntho e 'ngoe le e 'ngoe hape ho ba bohlokoa" e na le litšenyehelo tse sa lekanyetsoang. Ho etsa hore liphetoho tsa morao li se ke tsa khoneha ho etsa hore bille e se ke ea khoneha. Boleng ba litšenyehelo ke thepa ea state graph, eseng ea tlhokomeliso ea opareitara.

**Litšenyehelo:** schema migrations le reprocessing-on-purpose li thata ka boomo. Khoebo e amohelehang.

### 2. OCR le phetolelo li sebetsa ho LLM ea lehae, eseng API ea cloud

OCR: enjine ea open-source, Tesseract CLI fallback. Phetolelo + NER: Gemma ka Ollama, ho laptop ea Apple Silicon.

**Lebaka:** zero marginal cost ka tokomane; e ka hlahisoa hape (mohlala o tsitsitseng + li-prompt); 'me mohato oa ho lata o se o tlameha ho sebetsa ho tloha IP ea bolulo (mohloli o ka morao ho Akamai Bot Manager — `curl` e fumana 403), kahoo laptop e kenyelelitsoe.

**Litšenyehelo:** boleng ba phetolelo bo ka tlase ho mohlala oa frontier. Bakeng sa corpus ea reference moo Senyesemane sa mantlha se lulang se le ho feta ka ho tobetsa hang, seo sea lokile. Ha re bolele hore liphetolelo li na le matla.

### 3. Likarolo tse peli li arolelana sebopeho se le seng feela: bundle e phatlalalitsoeng

Pipeline ha e ngole ho database ea tlhahiso ka kotloloho. E hlahisa `{ SQL, asset manifest, cache-purge list }`. "Phatlalatso" = sebelisa bundle eo pele (push SQL ho edge SQL DB, sync assets ho object storage, purge the named cache keys).

**Lebaka:** lehlakore la lehae le lehlakore la edge li ka fetoha ka boikemelo; bundle e ka hlahlojoa; 'me "deploy data" e tšoana ka sebopeho nako le nako. The Worker ke app e nyane ea TypeScript/Hono — CSP e matla (ha ho `unsafe-inline`; inline JSON-LD e sha256-pinned), `Accept-Language` + naha→lipuisano tsa puo, cache ea leqephe la KV ea matsatsi a 30, cron ea ho hlokomela letsatsi le letsatsi — 'me ha e hloke ho tseba hore na data e entsoe joang.

**Litšenyehelo:** phetoho ea D1 schema e ama lifaele tse peli (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Inshorense e theko e tlaase.

### Lintlha tse sa lumelloeng tse kenyelelitsoeng boitšoarong

- Ha e amane le 'muso oa U.S.; ha ho lets'oao la molao.
- Li-redaction tsa mohloli lia bolokoa, ha li fetoloe.
- Video e fanoe ho DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` sebakeng sa marang-rang kaofela — e ka hlahlojoa ke lienjine tsa patlisiso, e sa kenyelelitsoe koetlisong ea AI.

E phela: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

