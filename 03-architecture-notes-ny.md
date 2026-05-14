# GitHub — Post 3 ya 3 · Zolemba za Architecture (Kukambirana ka ADR-style)

**Gwiritsani ntchito ngati:** Kukambirana pansi pa "Show and tell" / "Architecture", kapena `docs/` ADR seed.
**Keywords:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Chifukwa chake ufolens.com yapangidwa motere

Zolemba za zisankho zitatu zomwe zinapanga [ufolens.com](https://www.ufolens.com) (kuchititsidwa kotsatsitsidwa komanso kukhala m'zinenero zambiri kwa [PURSUE UAP archive](https://www.war.gov/ufo)). Ndondomeko/ndikambirana zilandiridwa.

### 1. Pipeline ndi forward-only state machine — mwadala

States: `discovered → downloaded → ocr_done → translated → published`. Chikalata chimayenda patsogolo kokha, ndipo chimachita zimenezi pokha ngati pali ntchito yoti chite. Zomwe zaperekedwa sizidzachititsidwanso (reprocessed) pokha ngati delta detector yaona kuti source yasinthidwa.

**Chifukwa:** OCR + translation ndizo zinthu zomwe zimadya ndalama, ndipo archive imakula nthawi zonse. Pipeline yomwe "imayambitsanso chilichonse kuti ikhale bwino" imakhala ndi mtengo wosatha. Kupanga kuti zisathe kubwerera m'mbuyo kumapangitsa kuti pasakhale bill yosatha. Ceiling ya mtengo ndi property ya state graph, osati ya kusamalira kwa operator.

**Mtengo:** schema migrations komanso reprocessing-on-purpose zimakhala zovuta mwadala. Ichi ndi tradeoff yomwe ingalandiridwe.

### 2. OCR ndi translation zimagwira ntchito pa local LLM, osati cloud API

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma kudzera mwa Ollama, pa laptop ya Apple Silicon.

**Chifukwa:** zero marginal cost pa chikalata chilichonse; reproducible (fixed model + prompts); ndipo gawo la fetch liyenera kukhala kuchokera ku residential IP (source ili pansi pa Akamai Bot Manager — `curl` imalandira 403), choncho laptop imakhala m'dongosolo kale.

**Mtengo:** khwaliti ya translation ili pansi pa frontier model. Koma pa reference corpus pomwe original English imakhala pafupi (click imodzi), zimenezi ndizo bwino. Sitikunena kuti translation zili zolondola kwambiri (authoritative).

### 3. Zigawo ziwiri zimagawana interface imodzi yokha: published bundle

Pipeline simalembetsa mwachindunji ku production database. Imatulutsa `{ SQL, asset manifest, cache-purge list }`. "Publishing" = apply that bundle forward (push SQL ku edge SQL DB, sync assets ku object storage, purge the named cache keys).

**Chifukwa:** mbali ya local komanso mbali ya edge zitha kukula zokha; bundle imatha kuonedwa (reviewable); ndipo "deploy data" imakhala ya mtundu umodzi nthawi zonse. Worker ndi TypeScript/Hono app ya pang'ono — strict CSP (palibe `unsafe-inline`; inline JSON-LD ili sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — ndipo siphunzirapo mmene data inapangidwira.

**Mtengo:** kusintha kwa D1 schema kumakhudza mafayilo awiri (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Ndi insurance yotsika mtengo.

### Zinthu zomwe sizingasinthidwe zomwe zaphatikizidwa m'makhalidwe

- Sili m'gulu la boma la U.S.; palibe zizindikiro zof 공식 (official insignia).
- Source redactions zisinthidwa, sizidzabweza.
- Video zaperekedwa kwa DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-wide — search-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1