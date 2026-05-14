# GitHub — Post 3 van 3 · Architectuurnotities (ADR-stijl Discussion)

**Gebruik als:** een Discussion onder "Show and tell" / "Architecture", of als basis voor een ADR in `docs/`.
**Trefwoorden:** architectuur, ADR, forward-only state machine, lokale LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Waarom ufolens.com is gebouwd zoals het is

Aantekeningen over de drie beslissingen die [ufolens.com](https://www.ufolens.com) (de doorzoekbare, meertalige herbouw van het [PURSUE UAP-archief](https://www.war.gov/ufo)) hebben gevormd. Commentaar / tegengas is welkom.

### 1. De pipeline is met opzet een 'forward-only' state machine

Staten: `discovered → downloaded → ocr_done → translated → published`. Een document beweegt alleen voorwaarts, en alleen als er werk te doen is. Gepubliceerde inhoud wordt nooit opnieuw verwerkt, tenzij een delta-detector ziet dat de bron daadwerkelijk is gewijzigd.

**Waarom:** OCR + vertaling zijn de dure operaties, en het archief groeit met de tijd. Een pipeline die "alles opnieuw draait voor de zekerheid" heeft onbegrensde kosten. Door achterwaartse overgangen onmogelijk te maken, wordt een op hol geslagen rekening onmogelijk. Het kostenplafond is een eigenschap van de state graph, niet van de waakzaamheid van de operator.

**Kosten:** schemamigraties en doelbewust herverwerken zijn opzettelijk onhandig. Een acceptabele afweging.

### 2. OCR en vertaling draaien op een lokale LLM, niet op een cloud-API

OCR: open-source engine, Tesseract CLI fallback. Vertaling + NER: Gemma via Ollama, op een Apple Silicon-laptop.

**Waarom:** nul marginale kosten per document; reproduceerbaar (vast model + prompts); en de fetch-stap moet al vanaf een residentieel IP-adres draaien (de bron staat achter Akamai Bot Manager — `curl` krijgt een 403), dus een laptop is sowieso al in het proces betrokken.

**Kosten:** de vertaalkwaliteit is lager dan die van een frontier-model. Voor een referentiecorpus waar het Engelse origineel altijd één klik verwijderd is, is dat prima. We beweren niet dat de vertalingen gezaghebbend zijn.

### 3. De twee helften delen exact één interface: een gepubliceerde bundel

De pipeline schrijft nooit rechtstreeks naar de productiedatabase. Het produceert een `{ SQL, asset manifest, cache-purge list }`. "Publiceren" = die bundel voorwaarts toepassen (SQL naar de edge SQL DB pushen, assets synchroniseren met object storage, de genoemde cache-sleutels purgen).

**Waarom:** de lokale kant en de edge-kant kunnen onafhankelijk van elkaar evolueren; de bundel is controleerbaar; en "data implementeren" heeft elke keer dezelfde vorm. De Worker is een kleine TypeScript/Hono-app — strikte CSP (geen `unsafe-inline`; inline JSON-LD is sha256-gepind), `Accept-Language` + land→taal-negotiatie, 30-dagen KV-paginacache, dagelijkse opschoon-cronjob — en hoeft nooit te weten hoe de data is gemaakt.

**Kosten:** een D1-schemawijziging raakt twee bestanden (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Goedkope verzekering.

### Niet-onderhandelbare punten ingebakken in het gedrag

- Niet gelieerd aan de Amerikaanse overheid; geen officiële insignes.
- Redacties in de bron worden behouden, nooit ongedaan gemaakt.
- Video toegeschreven aan DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` voor de hele site — indexeerbaar voor zoekmachines, afgemeld voor AI-scraping.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
