# GitHub — Post 3 fan 3 · Arsjitektuernotysjes (Diskusje yn ADR-styl)

**Brûk as:** in Diskusje ûnder "Show and tell" / "Arsjitektuer", of as begjin foar `docs/` ADR.
**Kaaiwurden:** arsjitektuer, ADR, allinich-foarút state machine, lokale LLM, Ollama, OCR, edge computing, CSP, feiligens-headers, data-pipeline, kosten-engineering, SQLite-manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Wêrom ufolens.com boud is sa't it is

Notysjes oer de trije beslissings dy't [ufolens.com](https://www.ufolens.com) (de sykjebere, meartalige werbou fan it [PURSUE UAP-argyf](https://www.war.gov/ufo)) foarmjûn hawwe. Kommentaar / tsjinspraak is wolkom.

### 1. De pipeline is in allinich-foarút state machine — mei sin

Steaten: `ûntdutsen → ynladen → ocr_klear → oerset → publisearre`. In dokumint giet allinich foarút, en allinich as der wurk te dwaan is. Publisearre ynhâld wurdt nea op 'e nij ferwurke, útsein as in delta-detektor sjocht dat de boarne echt feroare is.

**Wêrom:** OCR + oersetting binne de djoere operaasjes, en it argyf groeit oer tiid. In pipeline dy't "alles op 'e nij draait foar de feiligens" hat ûnbeheinde kosten. It ûnmooglik meitsjen fan weromgeande transysjes makket in troch it dak geande rekken ûnmooglik. It kostenplafond is in eigenskip fan de state graph, net fan de wachheid fan de operator.

**Kosten:** skema-migraasjes en mei sin op 'e nij ferwurkje binne mei opsetsin ûnhandich. Akseptabele ôfwaging.

### 2. OCR en oersetting rinne op in lokale LLM, net in wolk-API

OCR: iepenboarne-motor, Tesseract CLI-fallback. Oersetting + NER: Gemma fia Ollama, op in Apple Silicon-laptop.

**Wêrom:** nul marzjinale kosten per dokumint; reprodusearber (fêst model + prompts); en de hel-stap moat al rinne fan in partikulier IP-adres (de boarne sit achter Akamai Bot Manager — `curl` krijt in 403), dus in laptop is dochs al yn 't spul.

**Kosten:** de kwaliteit fan de oersetting is leger as dy fan in frontier-model. Foar in referinsjekorpus wêr't it orizjinele Ingelsk altyd mar ien klik fuort is, is dat prima. Wy beweare net dat de oersettings autoritatyf binne.

### 3. De twa helten diele presys ien ynterface: in publisearre bondel

De pipeline skriuwt nea direkt nei de produksjedatabase. It produsearret in `{ SQL, asset-manifest, cache-leegje-list }`. "Publisearjen" = dy bondel foarút tapasse (SQL nei de edge SQL DB triuwe, assets syngronisearje mei objekt-opslach, de neamde cache-kaaien leegje).

**Wêrom:** de lokale kant en de edge-kant kinne ûnôfhinklik evoluearje; de bondel is te besjen; en "data ynsette" hat eltse kear deselde foarm. De Worker is in lytse TypeScript/Hono-app — strange CSP (gjin `unsafe-inline`; inline JSON-LD is sha256-fêstmakke), `Accept-Language` + lân→taal-ûnderhanneling, 30-dagen KV-side-cache, deistige ûnderhâlds-cron — en it hoecht nea te witten hoe't de data makke is.

**Kosten:** in D1-skema-wiziging reitket twa bestannen (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Goedkeape fersekering.

### Net-ûnderhannelbere punten ynboud yn it gedrach

- Net ferbûn mei de Amerikaanske oerheid; gjin offisjele insignia.
- Boarne-redaksjes wurde bewarre, nea ûngedien makke.
- Fideo taskreaun oan DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` oer de hiele side — sykje-yndeksearber, AI-scrape-útskeakele.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
