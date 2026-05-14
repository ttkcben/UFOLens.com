# GitHub — Post 3 sur 3 · Dilancogo kunnafoniw (ADR-style Discussion)

**A baara kɛ i n’a fɔ:** Discussion "Show and tell" / "Architecture" kɔnɔ, walima `docs/` ADR daɲɛ fɔlɔ.
**Daɲɛw:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Nfa ye ufolens.com dilara cogo min na

Kunnafoniw minnu bɛ tali kɛ [ufolens.com](https://www.ufolens.com) ka laɲini saba la (min ye [PURSUE UAP kɔnɔkow](https://www.war.gov/ufo) ka ɲinini, kanw caman na). Kunnafoniw / pushback bɛ jaabi.

### 1. Pipeline ye forward-only state machine ye — a kɛra ka surun

Statew: `discovered → downloaded → ocr_done → translated → published`. Sɛbɛn bɛ taa ɲɛ dɔrɔn, wa ni baara bɛ kɛ. Lajɛli minnu lajɛra, olu tɛ kɛ kokura fo ni delta detector ye a ye ko fɔlɔ yɛlɛmana.

**Nfajɔ:** OCR + kalanko ye baara sarada ye, wa kɔnɔna bɛ bonya waati o waati. Pipeline min bɛ "bɛɛ kɛ kokura ka kɛ salimu ye" o sarada tɛ dan. Ka segin kɔ ka kɛ gɛlɛn ye, o bɛ wari dan gɛlɛya. Wari dan ye state graph ka fɛn ye, a tɛ operatɔri ka lajɛli ye.

**A sarada:** schema migrations ni reprocessing-on-purpose ka gɛlɛn. Yɛlɛma min bɛ se ka kɛ.

### 2. OCR ni kalanko bɛ kɛ LLM lokal na, a tɛ kɛ cloud API na

OCR: open-source engine, Tesseract CLI fallback. Kalanko + NER: Gemma via Ollama, Apple Silicon laptop kan.

**Nfajɔ:** sɛbɛn o sɛbɛn tɛ wari sara; a bɛ se ka kɛ kokura (model + prompts); wa fetch step ka kan ka kɛ ka bɔ residential IP la (fɔlɔ bɛ Akamai Bot Manager kɔfɛ — `curl` bɛ 403 sɔrɔ), o la, laptop bɛ loop kɔnɔ.

**A sarada:** kalanko kalite bɛ frontier model kɔfɛ. Kɔnɔna min na Angilɛkan fɔlɔ bɛ klik kelen dɔrɔn la, o ka fisa. An tɛ fɔ ko kalankow ye fanga ye.

### 3. Fɛn fila bɛɛ bɛ interface kelen dɔrɔn de fara ɲɔgɔn kan: lajɛli min lajɛra

Pipeline tɛ sɛbɛn kɛ production database la abada. A bɛ `{ SQL, asset manifest, cache-purge list }` bɔ. "Publishing" = o bundle lajɛ (SQL ci edge SQL DB la, assets sync object storage la, cache keys bɔ).

**Nfajɔ:** yɔrɔ-yɔrɔ ni edge bɛ se ka yɛlɛma yɛrɛmahɔrɔnya la; bundle bɛ se ka lajɛ; wa "deploy data" ye cogo kelen ye waati o waati. Worker ye TypeScript/Hono app fitinin ye — CSP ka gɛlɛn (tɛ `unsafe-inline`; inline JSON-LD ye sha256-pinned ye), `Accept-Language` + jamana→kan ɲɔgɔn sɔrɔ, tile 30 KV page cache, tile o tile housekeeping cron — wa a tɛna a dɔn abada data dilara cogo min na.

**A sarada:** D1 schema yɛlɛma bɛ tali kɛ filen fila la (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Assuransi nɔgɔman.

### Fɛn minnu tɛ se ka ɲɔgɔn sɔrɔ, olu bɛɛ kɔnɔ

- Tɛ tali kɛ Ameriki fanga la; fanga ka taama-shiya si tɛ.
- Sɛbɛnni fɔlɔw bɛ mara, u tɛ segin kɔ.
- Vidéo bɛ tali kɛ DVIDS / AARO la.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site bɛɛ la — ɲinini-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
