# GitHub — Dixi 3 dia 3 · Mikanda ya architecture (Makani ma ADR-style)

**Sadisa kala:** Makani ku nsi ya "Show and tell" / "Architecture", mba `docs/` ADR seed.
**Tanga yakwiza:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cinshi ufolens.com yapangidila ifi

Mikanda pa mambulu matatu mayala [ufolens.com](https://www.ufolens.com) (kuyanda, kumanyisa kwa laka ya zungulu kwa [PURSUE UAP archive](https://www.war.gov/ufo)). Ifyakuwana / kuyanda kwabwidi.

### 1. Pipeline kuli ngalu ya state machine ya kuyenda ku ntwala — kuli na lulumbi

States: `discovered → downloaded → ocr_done → translated → published`. Mukanda umosi uyenda fwayidi ku ntwala, na fwayidi kana kuli mulimo wa kusala. Mikanda yabula kana yasangulwasa diaka kana delta detector yamonanga kifunkula kwasaniki.

**Cinshi:** OCR + kumanyisa kuli ma opérations ma nzimbu, na archive iyaywiza mu nsungi. Pipeline yikweti "re-runs everything to be safe" yakweti nzimbu ya kana mifumbulu. Kuyalanga backward transitions zibwidi kwazola nzimbu ya kana mifumbulu ibwidi. Cost ceiling kuli property ya state graph, kana ya operator vigilance.

**Cost:** schema migrations na reprocessing-on-purpose kuli ngolo kwa kupanga. Tradeoff yabwidi.

### 2. OCR na kumanyisa misala ku local LLM, kana ku cloud API

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma via Ollama, ku laptop ya Apple Silicon.

**Cinshi:** zero marginal cost pa mukanda umosi; reproducible (fixed model + prompts); na fetch step ili kale na kusala ku residential IP (kifunkula kuli ku nsi ya Akamai Bot Manager — `curl` yakweti 403), nso laptop kuli mu loop anyway.

**Cost:** kumanyisa kwa quality kuli ku nsi ya frontier model. Ku reference corpus apo Ingileza ya kifunkula kuli fwayidi click umosi, kuli mbote. Kana twatekamena kumanyisa kuli na bukome.

### 3. Ifipande mibadi bikweti interface umosi fwayidi: bundle yabula

Pipeline kana yalembela ku production database directly. Yazola `{ SQL, asset manifest, cache-purge list }`. "Publishing" = kubwida bundle oyo ku ntwala (push SQL ku edge SQL DB, sync assets ku object storage, purge named cache keys).

**Cinshi:** local side na edge side zingakudisa independentemente; bundle ibwidi kuyanda; na "deploy data" kuli shape imo fwayidi mu nsungi ya yoso. Worker kuli app ya TypeScript/Hono ya yikadi — strict CSP (kana `unsafe-inline`; inline JSON-LD kuli sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — na kana yayanda zayi kima kimosi kya ngalu yakweti kupanga data.

**Cost:** D1 schema change miatala mikanda mibadi (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurance ya kikalakadi.

### Non-negotiables ma kubwida

- Kana ya nwanana na guvernu dia EUA; kana official insignia.
- Redakisa za kifunkula zisakwidi, kana zifungula.
- Video yatakama ku DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ku site ya yoso — search-indexable, AI-scrape-opted-out.

Kiakamwene: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
