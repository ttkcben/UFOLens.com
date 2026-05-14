# GitHub — Icipande 3 pali 3 · Ifya architecture (ADR-style Discussion)

**Cibomfiwe nga:** Ifyalandwe ifya "Show and tell" / "Architecture", nelyo `docs/` ADR seed.
**Amashiwi akulu:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cinshi ufolens.com yapangilwa ifi

Ifya kulemba pa mafumu yatatu ayapangile [ufolens.com](https://www.ufolens.com) (ukulekaisha, ukupilibula mu ndimi ishingi kwa [PURSUE UAP archive](https://www.war.gov/ufo)). Ifya kulabila / ifyabwesesha fyapokelelwa.

### 1. Pipeline kuli forward-only state machine — kuli ne lulumbi

States: `discovered → downloaded → ocr_done → translated → published`. Document ilaya libela, kabili fye nga kuli umulimo wa kubomba. Ifya kubilishiwa takapilibulwe nakabili kano delta detector ilamona source yalicinchika.

**Cinshi:** OCR + ukupilibula e milimo icindama, kabili archive ikula pa nshita. Pipeline iya "re-runs everything to be safe" yakwata cost ishafula. Ukuleka backward transitions ukuba impossible kulenga runaway bill ukuba impossible. Cost ceiling kuli property ya state graph, te ya operator vigilance.

**Cost:** schema migrations na reprocessing-on-purpose fyalilenga. Tradeoff yapokelelwa.

### 2. OCR na ukupilibula fipita pa local LLM, te pa cloud API

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma via Ollama, pa Apple Silicon laptop.

**Cinshi:** zero marginal cost pa document; reproducible (fixed model + prompts); kabili fetch step ili kale na ukubomba ukufuma pa residential IP (source kuli behind Akamai Bot Manager — `curl` ilakwata 403), nso laptop ili mu loop anyway.

**Cost:** ukunonsha kwa ukupilibula kuli pansi pa frontier model. Ku reference corpus apo original English ili fye click imo, ico nacibwino. Tatulelandapo ukuti ukupilibula kuli na bukome.

### 3. Ifipande fibili filabomfya interface imo fye: a published bundle

Pipeline takalembela ku production database directly. Ilafumya `{ SQL, asset manifest, cache-purge list }`. "Publishing" = ukubomfya iyo bundle forward (ukupush SQL ku edge SQL DB, ukuync assets ku object storage, ukuurge named cache keys).

**Cinshi:** local side na edge side fingakula independentemente; bundle ilakwata ukumonwa; kabili "deploy data" kuli shape imo fye pa nshita yonse. Worker kuli small TypeScript/Hono app — strict CSP (takwaba `unsafe-inline`; inline JSON-LD kuli sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — kabili takafwaye ukwishiba ifyo data yapangilwe.

**Cost:** D1 schema change ifibomba pa mafunde yabili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Cheap insurance.

### Non-negotiables baked into behaviour

- Takwaba ne filubo ne U.S. government; takwaba official insignia.
- Source redactions filapusuka, takapilibulwe.
- Video yatumbikwa ku DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` pa site yonse — search-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
