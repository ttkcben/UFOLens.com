# GitHub — Post 3 di 3 · Notis su la architeture (Discussion in stîl ADR)

**Ús come:** une discussion sot "Show and tell" / "Architecture", o come base par un ADR in `docs/`.
**Perpaulis clâf:** architeture, ADR, machine a stâts dome indevant, LLM locâl, Ollama, OCR, edge computing, CSP, intestazions di sigurece, pipeline di dâts, inzegnerie dai coscj, manifest SQLite, D1, R2, KV
**Leams ipertestuâi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Parcé che ufolens.com al è fat in cheste maniere

Notis su lis trê decisions che a àn plasmade [ufolens.com](https://www.ufolens.com) (la ricostruzion ricercjabil e multilengâl dal [archivi PURSUE UAP](https://www.war.gov/ufo)). Coments e critichis a son benvignûts.

### 1. La pipeline e je une machine a stâts dome indevant — fat apueste

Stâts: `discovered → downloaded → ocr_done → translated → published`. Un document si môf dome indevant, e dome cuant che al è lavôr di fâ. Il contignût publicât nol ven mai rielaborât, fûr che se un rileva-diferencis al viôt che la sorzint e je cambiade par da bon.

**Parcé che:** OCR e traduzion a son lis operazions plui costosis, e l'archivi al cres cul timp. Une pipeline che "e torne a fâ dut par sigurece" e à un cost cence limits. Rindi impussibilis lis transizions indaûr al rint impussibile une bole fûr di control. Il tet di spese al è une proprietât dal graf dai stâts, no de atenzion dal operadôr.

**Costi:** lis migrazions dal schem e la rielaborazion fate apueste a son deliberadementri malpratichis. Un compromès acetabil.

### 2. OCR e traduzion a funzionin su un LLM locâl, no su une API cloud

OCR: motôr open-source, fallback Tesseract CLI. Traduzion + NER: Gemma vie Ollama, su un portatil Apple Silicon.

**Parcé che:** cost marginâl zero par document; riproducibilitât (model e prompts fìs); e il pas di recuperi al à za di funzionâ di un IP residenziâl (la sorzint e je daûr di Akamai Bot Manager — `curl` al cjape un 403), duncje un portatil al è za in zîr.

**Costi:** la qualitât de traduzion e je sot di un model di avanguardie. Par un corpus di riferiment dulà che l'origjinâl inglês al è simpri a un clic di distance, al va ben. No pretendìn che lis traduzions a sedin autoritaris.

### 3. Lis dôs bandis a condividin esatementri une interface: un pachet publicât

La pipeline no scrîf mai diretementri tal database di produzion. E prodûs `{ SQL, manifest dai assets, liste di svuedâ la cache }`. "Publicâ" = aplicâ chel pachet indevant (meti il SQL tal DB SQL edge, sincronizâ i assets te memorie a ogjets, svuedâ lis clâfs de cache nomenadis).

**Parcé che:** la bande locâl e la bande edge a puedin evolvisi in maniere indipendente; il pachet al pues jessi controlât; e "implementâ i dâts" al à simpri la stesse forme. Il Worker al è une picele aplicazion TypeScript/Hono — CSP rigurose (nissun `unsafe-inline`; JSON-LD in linie blocât cun sha256), negoziazion `Accept-Language` + paîs→lenghe, cache de pagjine KV di 30 dîs, cron di manutenzion gjornalîr — e nol à mai bisugne di savê come che i dâts a son stâts fats.

**Costi:** une modifiche al schem D1 e tocje doi files (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Une assicurazion economiche.

### Robis no negoziabii integradis tal compuartament

- No afiliât cul guvier dai Stâts Unîts; nissune insegne uficiâl.
- Lis redazions de sorzint a son conservadis, mai anuladis.
- I videos a son atribuîts a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` par dut il sît — indiçizabil pe ricercje, esclûs de racuelte dâts pe AI.

Dal vîf: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
