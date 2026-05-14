# GitHub — Aselqan 3 deg 3 · Isefka n umecwar (Asegzi n ADR-style)

**Seqdec s:** Yiwen usegzi ddaw "Skan d asiwel" / "Amecwar", neɣ ADR seed n `docs/`.
**Awalen ileqqmen:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Isemtuyen:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Acuɣer i yettwabna ufolens.com s wudem-ines

Isefka ɣef tlata n isefka i yessuɣlen [ufolens.com](https://www.ufolens.com) (asemẓu yezmer ad yettunadi, s waṭas n tutlayin, n [PURSUE UAP archive](https://www.war.gov/ufo)). Iɣeblan / aseqqam ssexdamen.

### 1. Pipeline d yiwen n forward-only state machine — s tnemmara

Isefka: `discovered → downloaded → ocr_done → translated → published`. Yiwen ugur kan yettnerni ɣer zdat, d ugur kan m’ur yelli ara lecɣel ad yettwaxdem. Isefka yettwaɛedlen ur ttnarnint ara m’ur yelli ara delta detector yettwaɣay s isefka iẓṛiyan.

**Acuɣer:** OCR d aseqdec d lecɣel iǧehden, yerna amazzlu yettnerni deg wakud. Yiwen n pipeline yessuɣulen "re-runs everything to be safe" yesɛa tulay ur yelli ara. Asekles n transicions sserɣulen ad yettwaxdem ugur amagnu. Tulay n tulay d aseqdec n state graph, ur nelli ara n ugur n operator.

**Tulay:** schema migrations d reprocessing-on-purpose d lecɣel iǧehden. Aseqdec yelhan.

### 2. OCR d usegzi ttnarnint ɣef local LLM, ur nelli ara cloud API

OCR: open-source engine, Tesseract CLI fallback. Aseqdec + NER: Gemma via Ollama, ɣef yiwen n Apple Silicon laptop.

**Acuɣer:** zero marginal cost i yal isefru; yezmer ad yettwaxdem (fixed model + prompts); d step n fetch yessefk ad yettnerni seg yiwen n residential IP (amazzzlu yettwaɣay s Akamai Bot Manager — `curl` yettwaɣay 403), dɣa yiwen n laptop yettwaɣay deg uḥerrek.

**Aḥesfan:** quality n aseqdec d ddaw n frontier model. I yiwen n corpus anda Taglizit taẓṛit tella deg yiwen n tikli, d akken-iten yelha. Ur nesɛi ara aṭṭaf d aseqdec n isefka.

### 3. Sin n imuren ttemlilin deg yiwen n udlif: yiwen n bundle yettwaɛedlen

Pipeline ur yettaru ara ɣer production database s wudem amagnu. Yessuɣul `{ SQL, asset manifest, cache-purge list }`. "Publishing" = apply that bundle forward (push SQL ɣer edge SQL DB, sync assets ɣer object storage, purge the named cache keys).

**Acuɣer:** local side d edge side yezmer ad ttnarnint s wudem imagnu; bundle yezmer ad yettwazrew; d "deploy data" d yiwen n tilɣa yal tikli. Worker d yiwen n TypeScript/Hono app amectuḥ — strict CSP (ur yelli ara `unsafe-inline`; inline JSON-LD d sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — yerna ur teḥwiǧ ara ad tssin amek i yettwaxdem ugur.

**Aḥesfan:** yiwen n D1 schema change yettwaɣay s sin n isefka (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Timentilt tamectuḥt.

### Iḥebsiwen ur nelli ara d aseqqam i yettwabnan deg uḥerrek

- Ur yelli ara aṭṭaf d tḥukumt n Marikan; ur yelli ara isem n uwanak.
- Isefka iẓṛiyan yettwaḥṛan, ur ttnarnint ara.
- Vidyu yettwaɛeddel ɣer DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ɣef usmel amagnu — yezmer ad yettunadi, AI-scrape-opted-out.

Tizert: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
