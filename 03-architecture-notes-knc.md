# GitHub — Post 3 ta 3 · Architecture notes (ADR-style Discussion)

**A na taa yi kamar:** a Discussion "Show and tell" / "Architecture" goono, ko `docs/` ADR seed.
**Keywords:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com architecture ba ka'a ne

Notes decisions uku goono ka taa shaped [ufolens.com](https://www.ufolens.com) (searchable, multilingual rebuild [PURSUE UAP archive](https://www.war.gov/ufo) goono). Comments / pushback welcome.

### 1. Pipeline forward-only state machine no — on purpose

States: `discovered → downloaded → ocr_done → translated → published`. Document guda forward goono ka taa move, o a goono ka taa work buqata. Published content ba na taa reprocess ba sai ka delta detector source goono ka taa canza.

**Saboda:** OCR + translation expensive operations no, o archive goono ka taa grow over time. Pipeline wande "re-runs everything to be safe" goono ka taa unbounded cost no. Backward transitions ba na taa impossible goono, runaway bill impossible no. Cost ceiling property state graph goono no, ba operator vigilance.

**Cost:** schema migrations o reprocessing-on-purpose awkward no. Acceptable tradeoff.

### 2. OCR o translation local LLM goono ka taa run, ba cloud API

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma via Ollama, Apple Silicon laptop goono.

**Saboda:** zero marginal cost document guda ga; reproducible (fixed model + prompts); o fetch step ba ya buqata ka residential IP goono ka taa run (source Akamai Bot Manager ba no — `curl` 403 ka taa gane), to laptop goono loop goono no.

**Cost:** translation quality frontier model goono ka taa. Reference corpus goono English original click guda goono ka taa, wande fine no. Ba na taa claim ka translations authoritative no.

### 3. Halves biyu exactly interface guda goono ka taa share: published bundle

Pipeline ba na taa write production database goono directly ba. A `{ SQL, asset manifest, cache-purge list }` ka taa emit. "Publishing" = apply bundle wande forward (push SQL edge SQL DB ga, sync assets object storage ga, purge named cache keys goono).

**Saboda:** local side o edge side independent goono ka taa evolve; bundle reviewable no; o "deploy data" shape guda no time gaba. Worker small TypeScript/Hono app no — strict CSP (no `unsafe-inline`; inline JSON-LD sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — o ba ya buqata ka know ka data ba ka'a made no.

**Cost:** D1 schema change files biyu goono ka taa touch (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Cheap insurance.

### Non-negotiables baked into behaviour

- Ba ya affiliated U.S. government goono; ba ya official insignia.
- Source redactions preserved no, ba ya reversed.
- Video attributed DVIDS / AARO goono.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-wide — search-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

