# GitHub — Kwalo ya 3 mo go 3 · Ditlhotlho tsa Thulaganyo (Kgang ya ADR-style)

**Dirisa jaaka:** kgang ka fa tlase ga "Show and tell" / "Architecture", kgotsa `docs/` ADR seed.
**Mafoko a botlhokwa:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Dikgokagano:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Goreng ufolens.com e agilwe ka tsela e e leng ka yone

Ditlhotlho ka ditshwetso tse tharo tse di dirileng [ufolens.com](https://www.ufolens.com) (tlhagiso ya dipuo tse dintsi, e e patlisisegang ya [PURSUE UAP archive](https://www.war.gov/ufo)). Dikakgelo / pushback di a amogelesega.

### 1. Pipeline ke thulaganyo ya gago-fela ya maemo — ka boomo

Maemo: `discovered → downloaded → ocr_done → translated → published`. Tokomane e fela e tswelela pele, le fela fa go na le tiro ya go e dira. Dintlha tse di phatlaladitsweng ga di ke di busediwa gape ntle le fa delta detector e bona gore motswedi o fetogile.

**Goreng:** OCR + thaloso ke ditiro tse di turang, mme polokelo e gola ka nako. Pipeline e e "busetsang gape sengwe le sengwe go netefatsa" e na le ditlhwatlhwa tse di sa feleng. Go dira diphetogo tsa morago di sa kgonege go dira gore go se nne le molaetsa o o sa laolegeng. Boleng jwa ditlhwatlhwa ke karolo ya graph ya maemo, eseng ya tlhokomelo ya modirisi.

**Boleng:** diphetogo tsa schema le go busetsa gape ka boomo di thata ka boomo. Boleng jo bo amogelesegang.

### 2. OCR le thaloso di dirisiwa mo local LLM, eseng cloud API

OCR: open-source engine, Tesseract CLI fallback. Thaloso + NER: Gemma ka Ollama, mo laptop ya Apple Silicon.

**Goreng:** zero marginal cost ka tokomane; e kgona go busediwa (fixed model + prompts); le gape fetch step e setse e tshwanetse go dirisiwa go tswa go residential IP (motswedi o kwa morago ga Akamai Bot Manager — `curl` e bona 403), ka jalo laptop e mo sedikong le yone.

**Boleng:** boleng jwa thaloso bo kwa tlase ga model ya frontier. Go setlhopha sa ditshupetso kwa Seesemane sa ntlha se leng gaufi, seo se siame. Ga re kake ra re dithaloso di na le thata.

### 3. Dikgaolo tse pedi di dirisa segokaganyo se le sengwe fela: sephuthelwana se se phatlaladitsweng

Pipeline ga e ke e kwala mo production database ka tlhamalalo. E ntsha `{ SQL, asset manifest, cache-purge list }`. "Phatlalatso" = dirisa sephuthelwana seo pele (tswetsa SQL kwa edge SQL DB, kopanya assets kwa object storage, phepafatsa cache keys tse di umakilweng).

**Goreng:** local side le edge side di ka tlhabologa ka boikemelo; sephuthelwana se ka tlhatlhobiwa; le gape "deploy data" e na le seemo se se tshwanang ka dinako tsotlhe. The Worker ke TypeScript/Hono app e nnye — strict CSP (ga go `unsafe-inline`; inline JSON-LD e sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — mme ga e ke e tlhoka go itse gore datha e dirilwe jang.

**Boleng:** phetogo ya D1 schema e ama difaele tse pedi (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Inshorense e e theko e tlase.

### Dintlha tse di sa kgonegego tse di dirilweng mo boitshwarong

- Ga e amane le mmuso wa U.S.; ga go letshwao la semmuso.
- Dintlha tse di fitlhilweng tsa motswedi di bolokilwe, ga di ke di busediwa morago.
- Bidio e latofatsa DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` mo sethaleng sotlhe — e patlisisega ka di-injini tsa patlisiso, e kgaotse AI-scrape-opted-out.

E a tshediswa: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
