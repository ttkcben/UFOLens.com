# GitHub — 3/3 Argitalpena · Arkitektura oharrak (ADR estiloko eztabaida)

**Erabiltzeko modua:** "Show and tell" / "Architecture" ataleko Discussion gisa, edo `docs/` ADR hasiera gisa.
**Gako-hitzak:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hiperestekak:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Zergatik dagoen ufolens.com eraikita dagoen moduan

[ufolens.com](https://www.ufolens.com) ([PURSUE UAP artxiboaren](https://www.war.gov/ufo) berreraikuntza bilagarri eta eleaniztuna) moldatu zuten hiru erabakiei buruzko oharrak. Iruzkinak / kritikak ongi etorriak dira.

### 1. Pipeline-a aurreranzko soilik den egoera-makina bat da — nahita

Egoerak: `discovered → downloaded → ocr_done → translated → published`. Dokumentu bat aurrera bakarrik mugitzen da, eta lana dagoenean soilik. Argitaratutako edukia ez da inoiz berriro prozesatzen, delta detektagailu batek iturburua benetan aldatu dela ikusten ez badu.

**Zergatik:** OCR + itzulpena dira eragiketa garestienak, eta artxiboa denborarekin hazten da. "Dena berriro exekutatzen duen segurtasunagatik" pipeline batek kostu mugagabea du. Atzeranzko trantsizioak ezinezko egiteak faktura kontrolaezin bat ezinezko bihurtzen du. Kostuaren muga egoera-grafikoaren propietate bat da, ez operadorearen arretarena.

**Kostua:** eskema-migrazioak eta nahita egindako berriro prozesatzeak deserosoak dira apropos. Truke onargarria.

### 2. OCR eta itzulpena LLM lokal batean exekutatzen dira, ez hodeiko API batean

OCR: kode irekiko motorra, Tesseract CLI fallback-a. Itzulpena + NER: Gemma Ollama bidez, Apple Silicon eramangarri batean.

**Zergatik:** zero kostu marjinal dokumentu bakoitzeko; errepikagarria (eredu + prompt finkoak); eta fetch urratsak dagoeneko IP egoiliar batetik exekutatu behar du (iturburua Akamai Bot Manager-en atzean dago — `curl`-ek 403 bat jasotzen du), beraz, eramangarri bat prozesuan dago hala ere.

**Kostua:** itzulpenaren kalitatea punta-puntako eredu batena baino txikiagoa da. Jatorrizko ingelesa beti klik batera dagoen erreferentziazko corpus baterako, hori ondo dago. Ez dugu aldarrikatzen itzulpenak autoritatezkoak direnik.

### 3. Bi erdiek interfaze bakarra partekatzen dute: argitaratutako sorta bat

Pipeline-ak ez du inoiz zuzenean idazten ekoizpeneko datu-basean. `{ SQL, baliabide-manifestua, cache-garbiketa zerrenda }` igortzen du. "Argitaratzea" = sorta hori aurrera aplikatzea (SQL-a edge SQL DB-ra bultzatu, baliabideak objektuen biltegiratzearekin sinkronizatu, izendatutako cache-gakoak garbitu).

**Zergatik:** alde lokala eta edge aldea independenteki eboluziona daitezke; sorta berrikus daiteke; eta "datuak hedatzea" forma berekoa da beti. Worker-a TypeScript/Hono aplikazio txiki bat da — CSP zorrotza (`unsafe-inline` gabe; lerroko JSON-LD sha256-rekin finkatuta dago), `Accept-Language` + herrialde→hizkuntza negoziazioa, 30 eguneko KV orri-cachea, eguneroko mantentze-cron bat — eta ez du inoiz jakin behar nola sortu ziren datuak.

**Kostua:** D1 eskemaren aldaketa batek bi fitxategi ukitzen ditu (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Aseguru merkea.

### Portaeran txertatutako ezin negoziagarriak

- Ez dago AEBetako gobernuarekin afiliatuta; ez dago intsignia ofizialik.
- Iturburuko erredakzioak gordetzen dira, inoiz ez dira lehengoratzen.
- Bideoa DVIDS / AARO-ri egozten zaio.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` gune osoan — bilaketa-motorrek indexa dezakete, AI-scrape-tik kanpo utzita.

Zuzenean: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
