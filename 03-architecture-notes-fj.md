# GitHub — iVolau 3 vei 3 · iVolatukutuku ni iVakatakilakila (ADR-style Veivosaki)

**Na kenai vakayagataki:** me vaka e dua na Veivosaki ena "Show and tell" / "Architecture", se `docs/` ADR seed.
**Vosa bibi:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Na kena soqoni:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Na cava e tara kina na ufolens.com ena kena ivakarau oqo

Na veika e volai ena tolu na veika e digitaki ka bulia na [ufolens.com](https://www.ufolens.com) (na kena vakasaqaqarataki, e levu na kena vosa na kena taravi tale na [PURSUE UAP archive](https://www.war.gov/ufo)). E na ciqomi na veika e volai / veika e veibasai.

### 1. Na pipeline e dua na forward-only state machine — ena inaki

Na veika e tu: `discovered → downloaded → ocr_done → translated → published`. E na toso ga ki liu e dua na ivola, ka ni sa tiko na cakacaka me caka. Na veika e sa tabaki tu e na sega ni na cakacakataki tale vakavo ga ke sa kunea na delta detector ni sa veisau na ivurevure.

**Na cava:** Na OCR kei na vakadewa e na ka bibi sara, ka na tubu tiko na ivolatukutuku ena veigauna. E dua na pipeline e na "re-runs everything to be safe" e na sega ni rawa ni tiko na kena iwiliwili. Na kena sega ni rawa ni veisautaka na veika e sa caka e na vakadeitaka ni sega ni na levu na iwiliwili. Na kena iwiliwili ni veika e na vakayagataki e na toqai ena state graph, e sega ni na vakaraici mai vei ira na dauvakacakacaka.

**iWiliwili:** Na veisau ni schema kei na kena cakacakataki tale ena inaki e na ka dredre. E na ciqomi na kena veisau.

### 2. Na OCR kei na vakadewa e na cicivaki ena dua na local LLM, e sega ni dua na cloud API

OCR: open-source engine, Tesseract CLI fallback. Vakadewa + NER: Gemma ena Ollama, ena dua na Apple Silicon laptop.

**Na cava:** Sega ni levu na kena iwiliwili ni veika e na vakayagataki ena veivola taucoko; rawa ni vakatauvatani (fixed model + prompts); ka na dodonu me na cicivaki na fetch step mai na dua na residential IP (na ivurevure e tiko e daku ni Akamai Bot Manager — `curl` e na kunea na 403), me rawa ni tiko na laptop ena loma ni cakacaka.

**iWiliwili:** Na ituvaki ni vakadewa e na rauta na kena ituvaki ni frontier model. Vei ira na ivola e na rawa ni kunei na vosa vaka-Peritania ena dua ga na kena kiliki, e na yaga sara. E sega ni na tukuni ni dina na vakadewa.

### 3. Na rua na iwasewase e na wasea ga e dua na interface: na bundle e tabaki tu

Na pipeline e na sega ni vola sara ki na production database. E na vakauta `{ SQL, asset manifest, cache-purge list }`. "Vakalesu" = na kena vakayagataki na bundle oqori ki liu (biuta na SQL ki na edge SQL DB, veisemati na iyaya ki na object storage, vakasavasavataka na named cache keys).

**Na cava:** na vanua ni local kei na vanua ni edge e rawa ni veisau vakataki koya; na bundle e rawa ni vakaraici; ka na yaga na "deploy data" ena veigauna kece. Na Worker e dua na small TypeScript/Hono app — strict CSP (sega ni `unsafe-inline`; na inline JSON-LD e sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — ka sega ni na gadrevi me kila na kena buli na data.

**iWiliwili:** E dua na D1 schema change e na tarai rua na ivola (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Na inisiuranisi e rawarawa.

### Na veika e sega ni rawa ni veisautaki ena ivakarau ni cakacaka

- Sega ni veisemati kei na matanitu e Amerika; sega ni vakayagataka na kena ivakatakilakila.
- Na ivola dina e na maroroi, e sega ni na veisautaki.
- Na vidio e na dodonu me na qaqalo mai na DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ena vanua taucoko — search-indexable, AI-scrape-opted-out.

Bula tiko: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

