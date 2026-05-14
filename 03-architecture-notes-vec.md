# GitHub — Post 3 de 3 · Note de architetura (Discussion in stile ADR)

**Doparar come:** na Discussion soto "Show and tell" / "Architecture", o come base par un ADR in `docs/`.
**Parole ciave:** architetura, ADR, machina a stati soło in vanti, LLM local, Ollama, OCR, edge computing, CSP, header de sicuresa, pipeline de dati, ingegneria dei costi, manifest SQLite, D1, R2, KV
**Colegamenti ipertestuałi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Parché ufolens.com el xe costruìo come che'l xe

Note su łe tre decision che ga dà forma a [ufolens.com](https://www.ufolens.com) (la ricostruzion sercàbiłe e multilingue de l'archivio [PURSUE UAP](https://www.war.gov/ufo)). Comenti / critiche i xe benvegnùi.

### 1. El pipeline el xe na machina a stati soło in vanti — aposta

Stati: `scoverto → descargà → ocr_fato → tradoto → publicà`. Un documento el va soło in vanti, e soło quando che ghe xe lavoro da far. El contenudo publicà no'l vien mai rielaborà, a manco che un rilevador de delta no veda che la fonte ła xe canbià davero.

**Parché:** OCR + tradusion i xe łe operasion che costa, e l'archivio el cresse col tenpo. Un pipeline che "el rifà tuto par sicuresa" el ga un costo sensa limiti. Rèndar inposìbiłi łe transision a l'indrio el rende inposìbiłe na boleta che scapa de man. El teto de costo el xe na proprietà del grafo de stati, no de l'atension de chi ło dopara.

**Costo:** łe migrasion del schema e ła rielaborasion aposta łe xe deliberatamente scomode. Un conpromeso acetàbiłe.

### 2. OCR e tradusion i xira so un LLM local, no so n'API in cloud

OCR: motore open-source, Tesseract CLI de riserva. Tradusion + NER: Gemma via Ollama, so un portatiłe Apple Silicon.

**Parché:** costo marginal zero par documento; reprodusìbiłe (modèło e prompt fisa); e la fase de fetch ła ga xà da xirar da un IP residensial (la fonte ła xe proteta da Akamai Bot Manager — `curl` el ciapa un 403), quindi un portatiłe el xe xà coinvolto comuncue.

**Costo:** la qualità de ła tradusion ła xe soto un modèło de frontiera. Par un corpus de riferimento dove che l'original inglexe el xe senpre a un clic de distansa, va ben cusì. No se dixe che łe tradusion łe sia autèntiche.

### 3. Łe do metà łe condivide na soła interfacia: un bundle publicà

El pipeline no'l scrive mai diretamente sul database de produsion. El produxe `{ SQL, manifest dei asset, lista par svodar la cache }`. "Publicar" = aplicar quel bundle (mandar el SQL al DB SQL edge, sincronixar i asset so l'object storage, svodar łe ciavi de cache nominae).

**Parché:** la parte local e la parte edge łe pol evolverse in modo indipendente; el bundle el se pol revisar; e "deployar i dati" el ga senpre la stesa forma. El Worker el xe na picola aplicasion TypeScript/Hono — CSP severo (nesun `unsafe-inline`; el JSON-LD in linea el xe fisà co sha256), negosiasion `Accept-Language` + paese→łengua, cache de łe pagine in KV par 30 dì, cron giornaliero par la manutension — e no'l ga mai bisogno de saver come che i dati i xe stà fati.

**Costo:** un canbiamento al schema D1 el toca do file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). N'asicurasion che costa poco.

### Innegosiàbiłi integrà nel conportamento

- No afilià col governo dei Stati Unii; nesun stema ufisial.
- Łe censure fonte łe vien mantegnùe, mai roversàe.
- Video atribuìo a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` so tuto el sito — indicisàbiłe par la reserca, disativà par el scraping da parte de IA.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
