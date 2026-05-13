# GitHub — Post 3 of 3 · Architecture notes (ADR-style Discussion)

**Use as:** a Discussion under "Show and tell" / "Architecture", or `docs/` ADR seed.
**Keywords:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Why ufolens.com is built the way it is

Notes on the three decisions that shaped [ufolens.com](https://www.ufolens.com) (the searchable, multilingual rebuild of the [PURSUE UAP archive](https://www.war.gov/ufo)). Comments / pushback welcome.

### 1. The pipeline is a forward-only state machine — on purpose

States: `discovered → downloaded → ocr_done → translated → published`. A document only moves forward, and only when there's work to do. Published content is never reprocessed unless a delta detector sees the source actually changed.

**Why:** OCR + translation are the expensive operations, and the archive grows over time. A pipeline that "re-runs everything to be safe" has unbounded cost. Making backward transitions impossible makes a runaway bill impossible. The cost ceiling is a property of the state graph, not of operator vigilance.

**Cost:** schema migrations and reprocessing-on-purpose are deliberately awkward. Acceptable tradeoff.

### 2. OCR and translation run on a local LLM, not a cloud API

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma via Ollama, on an Apple Silicon laptop.

**Why:** zero marginal cost per document; reproducible (fixed model + prompts); and the fetch step already has to run from a residential IP (the source is behind Akamai Bot Manager — `curl` gets a 403), so a laptop is in the loop anyway.

**Cost:** translation quality is below a frontier model. For a reference corpus where the original English is always one click away, that's fine. We don't claim the translations are authoritative.

### 3. The two halves share exactly one interface: a published bundle

The pipeline never writes to the production database directly. It emits `{ SQL, asset manifest, cache-purge list }`. "Publishing" = apply that bundle forward (push SQL to the edge SQL DB, sync assets to object storage, purge the named cache keys).

**Why:** the local side and the edge side can evolve independently; the bundle is reviewable; and "deploy data" is the same shape every time. The Worker is a small TypeScript/Hono app — strict CSP (no `unsafe-inline`; inline JSON-LD is sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — and it never needs to know how the data was made.

**Cost:** a D1 schema change touches two files (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Cheap insurance.

### Non-negotiables baked into behaviour

- Not affiliated with the U.S. government; no official insignia.
- Source redactions are preserved, never reversed.
- Video attributed to DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-wide — search-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
