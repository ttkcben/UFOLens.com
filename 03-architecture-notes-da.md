# GitHub — Indlæg 3 af 3 · Arkitekturnoter (ADR-stil diskussion)

**Anvendelse:** En diskussion under "Vis og fortæl" / "Arkitektur", eller et udgangspunkt for `docs/` ADR.
**Nøgleord:** arkitektur, ADR, kun-fremadrettet tilstandsmaskine, lokal LLM, Ollama, OCR, edge computing, CSP, sikkerhedsheadere, datapipeline, omkostningsstyring, SQLite-manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Hvorfor ufolens.com er bygget, som det er

Noter om de tre beslutninger, der formede [ufolens.com](https://www.ufolens.com) (den søgbare, flersprogede genopbygning af [PURSUE UAP-arkivet](https://www.war.gov/ufo)). Kommentarer / modspil er velkomne.

### 1. Pipelinen er en kun-fremadrettet tilstandsmaskine — med vilje

Tilstande: `discovered → downloaded → ocr_done → translated → published`. Et dokument bevæger sig kun fremad, og kun når der er arbejde at udføre. Udgivet indhold genbehandles aldrig, medmindre en delta-detektor ser, at kilden rent faktisk har ændret sig.

**Hvorfor:** OCR + oversættelse er de dyre operationer, og arkivet vokser over tid. En pipeline, der "genkører alt for en sikkerheds skyld", har ubegrænsede omkostninger. At gøre tilbagegående overgange umulige gør en løbsk regning umulig. Omkostningsloftet er en egenskab ved tilstandsgrafen, ikke af operatørens årvågenhed.

**Omkostning:** skemamigreringer og bevidst genbehandling er bevidst besværlige. En acceptabel afvejning.

### 2. OCR og oversættelse kører på en lokal LLM, ikke en cloud-API

OCR: open source-motor, Tesseract CLI fallback. Oversættelse + NER: Gemma via Ollama, på en Apple Silicon-laptop.

**Hvorfor:** nul marginalomkostning pr. dokument; reproducerbart (fast model + prompts); og fetch-trinnet skal alligevel køre fra en privat IP (kilden er bag Akamai Bot Manager — `curl` får en 403), så en laptop er alligevel involveret.

**Omkostning:** oversættelseskvaliteten er under en state-of-the-art-model. For et referencekorpus, hvor den originale engelske tekst altid er et klik væk, er det fint. Vi hævder ikke, at oversættelserne er autoritative.

### 3. De to halvdele deler præcis én grænseflade: et udgivet bundt

Pipelinen skriver aldrig direkte til produktionsdatabasen. Den udsender `{ SQL, aktiv-manifest, cache-purge-liste }`. "Udgivelse" = anvend det bundt (push SQL til edge SQL DB, synkroniser aktiver til object storage, ryd de navngivne cache-nøgler).

**Hvorfor:** den lokale side og edge-siden kan udvikle sig uafhængigt; bundtet kan gennemgås; og "implementer data" har den samme form hver gang. Worker'en er en lille TypeScript/Hono-app — streng CSP (ingen `unsafe-inline`; inline JSON-LD er sha256-fastgjort), `Accept-Language` + land→sprog-forhandling, 30-dages KV-sidecache, dagligt vedligeholdelses-cron-job — og den behøver aldrig at vide, hvordan dataene blev skabt.

**Omkostning:** en D1-skemaændring berører to filer (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Billig forsikring.

### Ikke-forhandlingsbare principper indbygget i adfærden

- Ikke tilknyttet den amerikanske regering; ingen officielle emblemer.
- Kilde-redaktioner bevares, de fjernes aldrig.
- Video tilskrives DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` på hele sitet — søge-indekserbar, frameldt AI-scraping.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

