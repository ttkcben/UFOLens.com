# GitHub – Inlägg 3 av 3 · Arkitekturanteckningar (Diskussion i ADR-stil)

**Använd som:** en diskussion under "Show and tell" / "Architecture", eller som ett utkast för `docs/` ADR.
**Nyckelord:** arkitektur, ADR, framåtriktad tillståndsmaskin, lokal LLM, Ollama, OCR, edge computing, CSP, säkerhetsrubriker, datapipeline, kostnadsingenjörskonst, SQLite-manifest, D1, R2, KV
**Hyperlänkar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Varför ufolens.com är byggt som det är

Anteckningar om de tre beslut som formade [ufolens.com](https://www.ufolens.com) (den sökbara, flerspråkiga ombyggnaden av [PURSUE UAP-arkivet](https://www.war.gov/ufo)). Kommentarer / motargument är välkomna.

### 1. Pipelinen är en medvetet framåtriktad tillståndsmaskin

Tillstånd: `discovered → downloaded → ocr_done → translated → published`. Ett dokument rör sig bara framåt, och endast när det finns arbete att utföra. Publicerat innehåll ombearbetas aldrig om inte en deltadetektor ser att källan faktiskt har ändrats.

**Varför:** OCR + översättning är de dyra operationerna, och arkivet växer över tid. En pipeline som "kör om allt för säkerhets skull" har obegränsade kostnader. Att omöjliggöra bakåtgående övergångar gör en skenande räkning omöjlig. Kostnadstaket är en egenskap hos tillståndsgrafen, inte hos operatörens vaksamhet.

**Kostnad:** schemamigreringar och avsiktlig ombearbetning är medvetet krångliga. En acceptabel avvägning.

### 2. OCR och översättning körs på en lokal LLM, inte ett moln-API

OCR: öppen källkods-motor, Tesseract CLI som reserv. Översättning + NER: Gemma via Ollama, på en bärbar dator med Apple Silicon.

**Varför:** noll marginalkostnad per dokument; reproducerbart (fast modell + prompter); och hämtningssteget måste redan köras från en privat IP-adress (källan ligger bakom Akamai Bot Manager – `curl` får en 403), så en bärbar dator är ändå inblandad.

**Kostnad:** översättningskvaliteten är lägre än en toppmodern modell. För en referenskorpus där den engelska originaltexten alltid är ett klick bort är det okej. Vi hävdar inte att översättningarna är auktoritativa.

### 3. De två halvorna delar exakt ett gränssnitt: ett publicerat paket

Pipelinen skriver aldrig direkt till produktionsdatabasen. Den matar ut `{ SQL, tillgångsmanifest, lista för cache-rensning }`. "Publicering" = tillämpa det paketet framåt (skicka SQL till edge SQL DB, synkronisera tillgångar till objektlagring, rensa de namngivna cachenycklarna).

**Varför:** den lokala sidan och edge-sidan kan utvecklas oberoende av varandra; paketet är granskningsbart; och "driftsättningsdata" har samma form varje gång. Worker-appen är en liten TypeScript/Hono-app – strikt CSP (inget `unsafe-inline`; inbäddat JSON-LD är sha256-fäst), `Accept-Language` + land→språk-förhandling, 30-dagars KV-sidcache, daglig underhålls-cron – och den behöver aldrig veta hur datan skapades.

**Kostnad:** en D1-schemaändring påverkar två filer (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). En billig försäkring.

### Icke förhandlingsbara principer inbakade i beteendet

- Inte anslutet till den amerikanska regeringen; inga officiella insignier.
- Källredigeringar bevaras, återställs aldrig.
- Video tillskrivs DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` över hela webbplatsen – sökindexerbar, avanmäld från AI-skrapning.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
