# GitHub — Fasnin ya 3 ye 3 · Architekture gom-gom-minungã (ADR-style Discussion)

**Tũu Woto:** Naong-kaagre ("Show and tell" / "Architecture"), walla `docs/` ADR seed.
**Wɛɛg-yõdo:** architekture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Teeb-yikri:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bõe yĩnga n tags ufolens.com

Gom-gom-minungã n tags [ufolens.com](https://www.ufolens.com) (gom-gom pɛlɛg-yãkda, gom-gom tũudma ne [PURSUE UAP sɛb-yõdrã](https://www.war.gov/ufo)). Naong-kaagre / saglgo panga.

### 1. Pipeline kuli forward-only state machine — pids gom-gom-minungã

States: `discovered → downloaded → ocr_done → translated → published`. Sɛbã fãa fãa yikda, la gom-gom-minungã n yikda. Published content ka yĩnde n gũ-da n yikda n ka sɛbã yikda.

**Bõe yĩnga:** OCR la makins-pɛlɛgdgo ya gom-gom-minungã, la sɛb-yõdo pids gom-gom-minungã. Pipeline n "re-runs everything to be safe" yakwata cost ishafula. Ukuleka backward transitions ukuba impossible kulenga runaway bill ukuba impossible. Cost ceiling kuli property ya state graph, te ya operator vigilance.

**Cost:** schema migrations la reprocessing-on-purpose fyalilenga. Tradeoff yapokelelwa.

### 2. OCR la makins-pɛlɛgdgo yikda local LLM, ka yĩnde n gũ-da cloud API

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma via Ollama, pa Apple Silicon laptop.

**Bõe yĩnga:** zero marginal cost pa document; reproducible (fixed model + prompts); kabili fetch step ili kale na ukubomba ukufuma pa residential IP (source kuli behind Akamai Bot Manager — `curl` ilakwata 403), nso laptop ili mu loop anyway.

**Cost:** ukunonsha kwa ukupilibula kuli pansi pa frontier model. Ku reference corpus apo original English ili fye click imo, ico nacibwino. Tatulelandapo ukuti ukupilibula kuli na bukome.

### 3. Ifipande fibili filabomfya interface imo fye: a published bundle

Pipeline takalembela ku production database directly. Ilafumya `{ SQL, asset manifest, cache-purge list }`. "Publishing" = ukubomfya iyo bundle forward (ukupush SQL ku edge SQL DB, ukuync assets ku object storage, ukuurge named cache keys).

**Cinshi:** local side na edge side fingakula independentemente; bundle ilakwata ukumonwa; kabili "deploy data" kuli shape imo fye pa nshita yonse. Worker kuli small TypeScript/Hono app — strict CSP (takwaba `unsafe-inline`; inline JSON-LD kuli sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — kabili takafwaye ukwishiba ifyo data yapangilwe.

**Cost:** D1 schema change ifibomba pa mafunde yabili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Cheap insurance.

### Gom-gom-minungã

- Ka gom-gom-minungã ne U.S. government; ka gom-gom-minungã.
- Sɛbã yikda, ka yĩnde n gũ-da n yikda.
- Video yatumbikwa ku DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` pa site yonse — search-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
