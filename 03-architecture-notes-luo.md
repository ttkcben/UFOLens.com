# GitHub — Gikwanyisi mar 3 kuom 3 · Weche mag Chokruok (Discussion mar ADR-style)

**Ti kaka:** Kaka Discussion manie bwo "Show and tell" / "Architecture", kata `docs/` ADR seed.
**Weche Mokonyo:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Wanjruok mag Kony:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Gimomiyo ufolens.com ochor godo kaka en

Weche manie kinde mag gik duto ma ne ojawuok gi [ufolens.com](https://www.ufolens.com) (manyo gik duto, ma nigi dhok mang'eny, kendo gik mang'ado siri mag [PURSUE UAP archive](https://www.war.gov/ufo)). Weche kod gik manyien oyie.

### 1. Pipeline en state machine ma dhi nyime — gi gima dwarore

States: `discovered → downloaded → ocr_done → translated → published`. Gik mang'ado siri duto dhi nyime, kendo mana ka nigi tich ma odwarore. Gik ma osemieng'ore ok onyal manyore kendo ka delta detector ok one gi wang' apaa.

**Gimomiyo:** OCR kod lendo gin gik ma ochor godo, kendo gik mang'ado siri duto dhi nyime. Pipeline ma "re-runs everything to be safe" nigi gik mang'ado siri duto ma ok ochomo. Timore gi gik mang'ado siri duto ma ok ochomo kendo ok ochomo. Gik duto ochor godo kaka en.

**Gik duto:** schema migrations kod reprocessing-on-purpose gin gik ma ok ochomo apaa. Oyie.

### 2. OCR kod lendo timore gi LLM mar gweng', ok gi cloud API

OCR: engine mar open-source, Tesseract CLI fallback. Translation + NER: Gemma via Ollama, e laptop mar Apple Silicon.

**Gimomiyo:** zero marginal cost per document; reproducible (fixed model + prompts); kendo fetch step nigi gi residential IP (the source is behind Akamai Bot Manager — `curl` gets a 403), kendo laptopni nigi e iye.

**Gik duto:** translation quality nigi e bwo frontier model. Kaka gik mang'ado siri duto nigi e iye, en kanyachiel. Ok wadwar ni translation obed gi gik mang'ado siri duto.

### 3. Gik duto otin'g'o interface achiel: bundle ma osemieng'ore

Pipeline ok onyal wuok e database mar ji duto apaa. Oluwo `{ SQL, asset manifest, cache-purge list }`. "Publishing" = manyo bundle ni (`push SQL to the edge SQL DB, sync assets to object storage, purge the named cache keys`).

**Gimomiyo:** gweng' mar gweng' kod gweng' mar gweng' nyalo bedo gi gik manyien ma ok ochomo; bundle ni inyalo manyo; kendo "deploy data" nigi gi gik mang'ado siri duto ma ok ochomo. Workerni en app mar TypeScript/Hono matin — strict CSP (no `unsafe-inline`; inline JSON-LD is sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — kendo ok onyal ng'eyo kaka data ne ochor godo.

**Gik duto:** D1 schema change otin'g'o fail ariyo (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurance ma ok ochomo.

### Gik ma ok onyal bedo gi gik manyien ma ok ochomo

- Ok oton'g' gi sirkal mar U.S.; ok onyal bedo gi ranyisi moro amora mar sirkal.
- Gik mang'ado siri duto gin ma ok ochomo, ok onyal lokore apaa.
- Vidio ochieng'ore gi DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` manie site duto — search-indexable, AI-scrape-opted-out.

Mangima: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

