# GitHub — Post 3 de 3 · Notes de architetura (discussion de stil ADR)

**Dovrar coche:** na discussion sot "Show and tell" / "Architecture", o coche basa per n ADR te `docs/`.
**Paroles clau:** architetura, ADR, maschina a stac ma inant, LLM local, Ollama, OCR, edge computing, CSP, security headers, data pipeline, ingegneria di cosć, manifest SQLite, D1, R2, KV
**Coleganc:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Perchël che ufolens.com ie fat su coche l ie

Notes sun les trëi dezijions che à dat forma a [ufolens.com](https://www.ufolens.com) (la re-costruzion consultabla y plurilingue dl [archif PURSUE UAP](https://www.war.gov/ufo)). Cumënc / critiga ie bënunì.

### 1. La pipeline ie na maschina a stac ma inant — fat aposta

Stac: `scuprì → dejarià → ocr_fat → tradot → publicà`. N document se move ma inant, y ma canche l ie velch de fé. L contignù publicà ne vën mei rielaborà, sce n rilevator de delta no vëiga che la surant se à propi mudà.

**Perchël:** OCR + traduzion ie les operazions che costa, y l'archif crësc col tëmp. Na pipeline che "re-fé dut per segurëza" à n cust zënza fin. Rënder i pass de reviers impuscibl rënd na spëisa fora de cuntrol impuscibla. L limit massim di cosć ie na carateristica dl graf di stac, y nia dla vijilanza dl operator.

**Cust:** migrazions de schema y rielaborazions fates aposta ie deliberatamënter nia saurides. N cumpromes azetabl.

### 2. OCR y traduzion va sun n LLM local, y nia sun na API de cloud

OCR: motor open-source, fallback cun Tesseract CLI. Traduzion + NER: Gemma tres Ollama, sun n laptop Apple Silicon.

**Perchël:** cust marginal a zero per document; reproduzibl (model fissà + prompts); y l pass de ciafè i dac muessa bele unì fat da n IP residenzial (la surant ie do Akamai Bot Manager — `curl` giapa n 403), perchël ie n laptop bele tl prozes.

**Cust:** la qualità dla traduzion ie sot a n model de frontiera. Per n corpus de referimënt ulà che l'original nglëisc ie forsc ma a n click de distanza, va chësc bën. Ne dijons nia che les traduzions ie autoritevules.

### 3. Les doi perts spartësc ma n'interfaza: n bundle publicà

La pipeline ne scrij mei diret tla banca de dac de produzion. La dà ora n `{ SQL, manifest di assets, lista de cache-purge }`. "Publiché" uel dì apliché chël bundle inant (spenjer l SQL tla banca de dac SQL de bòrd, sincronisé i assets tl object storage, purifiché les cles de cache nominedes).

**Perchël:** la pert locala y la pert de bòrd possa se svilupé independentemënter; l bundle possa unì controlà; y "mëter online dac" à forsc la medema forma. L Worker ie na pitla app en TypeScript/Hono — CSP strent (no `unsafe-inline`; JSON-LD inline ie fissà cun sha256), negoziazion de `Accept-Language` + paesc→rujeneda, na cache de plates KV de 30 dis, n cron de mantenimënter al di — y no l ie mei debujen che l sàpie coche i dac ie unic fac.

**Cust:** n mudamënt al schema de D1 reverda doi files (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). N'assicurazion a bon marcià.

### Ponts nia tratabli tegnic tl cumportamënt

- Nia afilià al guviern di Stac Unic; degun segn ufizial.
- Les redazions dla surant vën mantenides y mei revochedes.
- Video atribuì a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` sun dut l sit — da indizé dai muteres de inrescida, cherdà ora dal scraping de AI.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

