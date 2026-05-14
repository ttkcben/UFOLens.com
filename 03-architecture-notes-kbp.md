# GitHub — Post 3 of 3 · Architecture notes (ADR-style Discussion)

**Tɔzɩ kpee nɛ:** Tɔm ("Show and tell" / "Architecture"), yaa `docs/` ADR seed.
**Yaaŋ hɔɔlɩŋ:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Lanaa taa kpaŋŋa:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com gbɔŋgɔrtoo

[ufolens.com](https://www.ufolens.com) (PURSUE UAP archive taa tɔm yɔɔŋ gbɔŋgɔrtoo) lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo. Comments / pushback kɛlɛkɛlɛ.

### 1. Pipeline lɛ, forward-only state machine — ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo

States: `discovered → downloaded → ocr_done → translated → published`. Document lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo. Published content lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo. Delta detector lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo.

**Gbɔŋgɔrtoo:** OCR + translation lɛ, expensive operations, nɛ archive lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo. Pipeline lɛ, unbounded cost. Backward transitions impossible lɛ, runaway bill impossible. Cost ceiling lɛ, state graph ɛ-property, not of operator vigilance.

**Cost:** schema migrations and reprocessing-on-purpose lɛ, deliberately awkward. Acceptable tradeoff.

### 2. OCR and translation run on a local LLM, not a cloud API

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma via Ollama, on an Apple Silicon laptop.

**Gbɔŋgɔrtoo:** zero marginal cost per document; reproducible (fixed model + prompts); and the fetch step already has to run from a residential IP (source lɛ, Akamai Bot Manager ɛ-taa — `curl` gets a 403), so a laptop is in the loop anyway.

**Cost:** translation quality lɛ, frontier model ɛ-below. Reference corpus lɛ, original English is always one click away, that's fine. We don't claim the translations are authoritative.

### 3. The two halves share exactly one interface: a published bundle

Pipeline lɛ, ɛ-production database taa tɔm yɔɔŋ gbɔŋgɔrtoo tɩkpɛŋŋŋŋ. It emits `{ SQL, asset manifest, cache-purge list }`. "Publishing" = apply that bundle forward (push SQL to the edge SQL DB, sync assets to object storage, purge the named cache keys).

**Gbɔŋgɔrtoo:** local side nɛ edge side lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo; bundle lɛ, reviewable; and "deploy data" lɛ, same shape every time. Worker lɛ, small TypeScript/Hono app — strict CSP (no `unsafe-inline`; inline JSON-LD is sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — and it never needs to know how the data was made.

**Cost:** D1 schema change touches two files (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Cheap insurance.

### Non-negotiables baked into behaviour

- Not affiliated with the U.S. government; no official insignia.
- Source redactions are preserved, never reversed.
- Video attributed to DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-wide — search-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

