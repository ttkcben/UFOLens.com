# GitHub — Pos 3 vaan 3 · Aonteikeninge euver architectuur (ADR-stijl Discussie)

**Te gebruke es:** 'n Discussie oonder "Show and tell" / "Architectuur", of es oetgaankspunt veur `docs/` ADR.
**Trefwäörd:** architectuur, ADR, allein-nao-veure-staotsmachine, lokale LLM, Ollama, OCR, edge computing, CSP, beveiligingsheaders, datapipeline, koste-engineering, SQLite-manifes, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Wierum ufolens.com is gebouwd wie 't is gebouwd

Aonteikeninge euver de drei beslissinge die [ufolens.com](https://www.ufolens.com) ('de doorzeukbare, meertalige herbouw vaan 't [PURSUE UAP-archief](https://www.war.gov/ufo)) gevörmp höbbe. Commintaar / tegegas is wèlkome.

### 1. De pipeline is 'n allein-nao-veure-staotsmachine — mèt opzat

Staote: `ontdek → gedownload → ocr_klaor → vertaald → gepubliceerd`. E document beweeg allein nao veure, en allein es 'r wèrk te doen is. Gepubliceerde content weurt noets obbenuits verwerk, tenzij 'nen delta-detector zuut tot de bron ech veranderd is.

**Wierum:** OCR + vertaoling zien de deuste operaties, en 't archief greujt mèt d'n tied. 'n Pipeline die 'alles obbenuits deit veur de zekerheid' heet oonbegrensde koste. Door trökstappe oonmeugelek te make, weurt 'n oet de hand loupende rekening oonmeugelek. 't Kosteplafong is 'n eigesjap vaan de staotsgraaf, neet vaan de waakzaamheid vaan d'n operator.

**Koste:** schemamigraties en mèt opzat obbenuits verwerke zien bewus lesteg. 'nen Acceptabele concessie.

### 2. OCR en vertaoling drejje op 'ne lokale LLM, neet op 'n cloud-API

OCR: open-source-engine, Tesseract CLI-fallback. Vertaoling + NER: Gemma via Ollama, op 'nen Apple Silicon-laptop.

**Wierum:** nul marginale koste per document; reproduceerbaar (vas model + prompts); en de ophaalstap mós al vaanaof e residentieel IP-adres drejje (de bron zit achter Akamai Bot Manager — `curl` krijg 'ne 403), dus 'ne laptop is sowieso al in 't speul.

**Koste:** vertaolkwaliteit is minder es 'n topmodel. Veur 'n referentiecorpus boe 't oersprunkelek Ingels altied mer eine klik eweg is, is dat prima. Veer bewaere neet tot de vertaolinge bindend zien.

### 3. De twie helfte deile exak ein interface: 'ne gepubliceerde bundel

De pipeline sjrijf noets direk nao de productiedatabase. Ze produceert `{ SQL, asset-manifes, cache-opsjoonlies }`. 'Publicere' = dee bundel nao veure touwpasse (SQL nao de edge-SQL-DB pushe, assets synchronisere nao object-opslag, de geneumde cache-sleutele opsjone).

**Wierum:** de lokaal kant en de edge-kant kinne oonaofhenkelek vaan mekaar evoluere; de bundel is te controlere; en 'data inzette' heet edere kier dezelfde vörm. De Worker is 'n klein TypeScript/Hono-app — strikte CSP (gein `unsafe-inline`; inline JSON-LD is mèt sha256-gepind), `Accept-Language` + land→taol-oonderhandeling, 30-daagse KV-pagina-cache, daaglikse oonderhoudscron — en ze hoof noets te wete wie de data is gemaak.

**Koste:** 'n D1-schemaverandering raak twie bestande (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Gojekoupe verzekering.

### Oonoontkoombaarhede die in 't gedraag ingebakke zitte

- Neet geaffilieerd mèt de Amerikaanse euverheid; gein officieel logo's.
- Redacties oet de bron blieve behawwe, weure noets oongedoon gemaak.
- Video touwgesjreve aon DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-breed — zeuk-indexeerbaar, aofgemeld veur AI-scraping.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
