# GitHub — Khasi ya 3 ya 3 · Maṅwalo a magandavhelo (ADR-style Discussion)

**Shumisa sa:** Discussion fhasi ha "Show and tell" / "Architecture", kana `docs/` ADR seed.
**Maipfi a ndeme:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Zwiṱhuṱhisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ndi ngani ufolens.com yo fhaṱwa nga yeneyi nḓila

Maṅwalo nga ha dzi nḓila tharu dzo vhumbaho [ufolens.com](https://www.ufolens.com) (u fhaṱulula ha mimuthu, ho vuleaho ha [vhulungelo ha PURSUE UAP](https://www.war.gov/ufo)). Comments / pushback zwi ṱanganedziwa.

### 1. Pipeline ndi forward-only state machine — nga u ṱoḓa

States: `discovered → downloaded → ocr_done → translated → published`. Maṅwalo a fambela phanḓa fhedzi, nahone fhedzi musi hu na mushumo wa u shanduka. Zwiṱhalutshedzeli zwo phablishiwaho a zwi dovhi zwa lungiswa nga nnḓa ha musi delta detector i tshi vhona uri tshitalo tsho shanduka.

**Ndi ngani:** OCR + u ṱalutshedzela ndi mushumo wa maḓi, nahone vhulungelo vhu a hula nga tshifhinga. Pipeline ine "ya dovha ya shanda zwoṱhe uri hu vhe hu si na khombo" i na cost i sa fhele. U sa kona u ita backward transitions zwi ita uri bill i sa koni u fhela i sa konadzei. Cost ceiling ndi property ya state graph, hu si ya u sedza ha operator.

**Cost:** schema migrations na reprocessing-on-purpose ndi zwa u ḓiṱoḓela zwi sa ṱanganedzei. Tradeoff yo ṱanganedzwaho.

### 2. OCR na u ṱalutshedzela zwi shanda kha local LLM, hu si cloud API

OCR: open-source engine, Tesseract CLI fallback. U ṱalutshedzela + NER: Gemma nga Ollama, kha Apple Silicon laptop.

**Ndi ngani:** zero marginal cost per document; i konaho u dovha ya shandiswa (fixed model + prompts); nahone fetch step yo no fanela u shanda u bva kha residential IP (tshitalo tshi murahu ha Akamai Bot Manager — `curl` i wana 403), ngauralo laptop i vhukati ha mushumo.

**Cost:** translation quality i fhasi ha frontier model. Kha reference corpus hune English ya mathomo i nga wanala nga u clicka nthihi, zwo luga. A ri ḓivhadzi uri dzi ṱhalutshedzeli dzi na nungo.

### 3. Zwipindwana zwivhili zwi share exactly one interface: bundle yo phablishiwaho

Pipeline a i ṅwali kha production database nga ho livhaho. I bvisa `{ SQL, asset manifest, cache-purge list }`. "U phablisha" = u shumisa bundle yeneyo phanḓa (u sendela SQL kha edge SQL DB, u sync assets kha object storage, u purge named cache keys).

**Ndi ngani:** the local side na the edge side zwi nga hula nga u ḓiimisela; the bundle ndi reviewable; nahone "deploy data" i na vhuimo ho fanaho tshifhinga tshoṱhe. The Worker ndi app ntswu ya TypeScript/Hono — strict CSP (a hu na `unsafe-inline`; inline JSON-LD ndi sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — nahone a i faneli u ḓivha uri data yo itwa hani.

**Cost:** D1 schema change i kwama zwikhala zwivhili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurance i si ya maḓi.

### Zwi sa koni u shanduka zwo vhewa kha maitele

- A yo ngo ṱangana na muvhuso wa U.S.; a hu na zwiṱingandevhe zwa vhumuvhuso.
- Source redactions zwo vhulungwa, a zwi phethedzelwi.
- Vidio yo ṋewa DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-wide — search-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

