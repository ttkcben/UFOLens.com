# GitHub — Aselqan 3 seg 3 · Isentel n tneɣlin (ADR-style Discussion)

**Aseqdac:** Isentel deg "Aglam d usken" / "Tineɣlin", neɣ `docs/` ADR seed.
**Awalen isennanen:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Isemtelen:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Acuɣer i yettwakker ufolens.com s wazal-agi

Isentel ɣef tẓeɣ n tmuɣliwin yellan deg [ufolens.com](https://www.ufolens.com) (asemlal yettnadiyen, isgadanen deg waṭas n tutlayin n [PURSUE UAP archive](https://www.war.gov/ufo)). Isentel / pushback marḥaba.

### 1. Pipeline d asensel n usentel s yiwet n tɣuri — s umenṭag

Isentel: `discovered → downloaded → ocr_done → translated → published`. Asemalal yettamɣel ala ɣer zdat, yerna ala deg wakud ideg yella umahil. Asemalal yettwassuffɣen ur yettwamɣel ara tikelt nniḍen alamma isbeddel n ubeddel yettwassen deg uɣbalu yettubeddel.

**Acuɣer:** OCR + asuqqel d imahilen ɣlayen, yerna ammud yettamɣel deg wakud. A pipeline yettamɣen "ad yettamɣel yal aɣbalu i wasfel" ur yelli ara iswir n iswir. Asefsay n tiggiwin ur yelli ara yettwasbedd yettagarant-d ur yelli ara abeddel n idrimen. Iswir n iswir d tiggi n usentel, ur yelli ara d tiggi n uḥakim.

**Isafen:** schema migrations d reprocessing-on-purpose d imahilen isemḍelen. Asefsay n isafen.

### 2. OCR d asuqqel ttamɣen deg local LLM, ur yelli ara deg cloud API

OCR: open-source engine, Tesseract CLI fallback. Asuqqel + NER: Gemma via Ollama, deg Apple Silicon laptop.

**Acuɣer:** ur yelli ara iswir n iswir n idrimen i yal isemlal; yettwasbedd (fixed model + prompts); yerna fetch step ilaq ad yamɣel seg residential IP (aɣbalu yella deg Akamai Bot Manager — `curl` yettak 403), dɣa a laptop yella deg ulɣu.

**Isafen:** aswir n asuqqel yella deg iswir n umuddir n wasentel. I ummud n tmuɣli ideg Taglizit tamezwarut tella deg yiwet n tɣuri, d ayen yelhan. Ur nettales ara asuqqel d isebḍalen.

### 3. Sin n imuren ttemlilit deg yiwet n tfelwit: a bundle yettwassuffɣen

Pipeline ur yettaru ara ɣer production database s umuddir. Yettazenz-d `{ SQL, asset manifest, cache-purge list }`. "Asuffeɣ" = aseggas n bundle ɣer zdat (aseggas n SQL ɣer edge SQL DB, sync assets ɣer object storage, purge n cache keys yettwasemman).

**Acuɣer:** aɣbalu n umkan d aɣbalu n edge yezmer ad yettwabeddlen s tẓeɣ; bundle yezmer ad yettwasbedd; yerna "deploy data" d yiwet n tmuɣli yal tikelt. Worker d aɣbalu ameẓyan n TypeScript/Hono — strict CSP (ur yelli `unsafe-inline`; inline JSON-LD d sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — yerna ur yettḥwaǧ ara ad yessen amek i yettwaxdem isefka.

**Isafen:** a D1 schema change yettamɣel sin n isemlalen (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurance ɣlayen.

### Ur yelli ara imahilen yettwasbedden deg wasensel

- Ur yettwassegzay ara ɣer U.S. government; ur yelli ara iferdisen n udabu.
- Tilɣin yettwaseslen n uɣbalu ttwasbedden, ur ttnifrin ara.
- Tisin n tmuɣli tettekkes ɣer DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-wide — yettwasen deg search-indexable, AI-scrape-opted-out.

Tussna: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
