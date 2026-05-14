# GitHub — Chigawo 3 pa 3 · Zolemba zamapangidwe (Zokambirana za mtundu wa ADR)

**Gwiritsani ntchito ngati:** Zokambirana pansi pa "Sonyezani ndi kufotokoza" / "Mapangidwe", kapena `docs/` ADR seed.
**Mawu ofunikira:** mapangidwe, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Ma hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Chifukwa chake ufolens.com yamangidwa momwe ilili

Zolemba pa zisankho zitatu zomwe zidapanga [ufolens.com](https://www.ufolens.com) (kuphatikizanso kwachilankhulo chambiri, kosakika kwa [PURSUE UAP archive](https://www.war.gov/ufo)). Ndemanga / kutsutsa ndikolandiridwa.

### 1. Pipi ndi makina a state machine omwe amangopita patsogolo — mwadala

Mkhalidwe: `discovered → downloaded → ocr_done → translated → published`. Chikalata chimangopita patsogolo, ndipo pokhapokha ngati pali ntchito yochita. Zomwe zasindikizidwa sizidzasinthidwanso pokhapokha ngati delta detector ipeza kuti gwero lasintha.

**Chifukwa chake:** OCR + kumasulira ndi ntchito zodula, ndipo archive imakula pakapita nthawi. Pipi yomwe "imayendetsanso chilichonse kuti ikhale yotetezeka" imakhala ndi mtengo wosatha. Kupangitsa kusinthika kwakumbuyo kukhala kosatheka kumapangitsa kuti bilu yosalamulirika ikhale yosatheka. Malire a mtengo ndi katundu wa state graph, osati tcheru cha wogwira ntchito.

**Mtengo:** schema migrations ndi reprocessing-on-purpose ndizovuta mwadala. Ndi kusinthanitsa kovomerezeka.

### 2. OCR ndi kumasulira zimayendetsa pa LLM yakumaloko, osati cloud API

OCR: injini ya open-source, Tesseract CLI fallback. Kumasulira + NER: Gemma kudzera pa Ollama, pa laputopu ya Apple Silicon.

**Chifukwa chake:** palibe mtengo wowonjezera pa chikalata chilichonse; wobwereza (fixed model + prompts); ndipo sitepe yofufuzira iyenera kuyendetsa kuchokera ku residential IP (gwero lili kumbuyo kwa Akamai Bot Manager — `curl` imapeza 403), kotero laputopu ili mu loop nthawi zonse.

**Mtengo:** ubwino wa kumasulira uli pansi pa frontier model. Kwa corpus yofotokozera kumene Chingerezi choyambirira nthawi zonse chimakhala kudina kumodzi, ndikwabwino. Sititsutsa kuti kumasulira ndi kovomerezeka.

### 3. Magawo awiri amagawana mawonekedwe amodzi: bundle yosindikizidwa

Pipi siilemba ku database yopangira mwachindunji. Imatulutsa `{ SQL, asset manifest, cache-purge list }`. "Kusindikiza" = gwiritsani ntchito bundle patsogolo (ikankhire SQL ku edge SQL DB, sync zida ku object storage, chotsani makiyi a cache otchulidwa).

**Chifukwa chake:** mbali yakumaloko ndi mbali ya edge zikhoza kusinthika payekha; bundle imatha kuwunikidwa; ndipo "deploy data" imakhala yofanana nthawi zonse. Worker ndi pulogalamu yaing'ono ya TypeScript/Hono — strict CSP (palibe `unsafe-inline`; inline JSON-LD ndi sha256-pinned), `Accept-Language` + dziko→chilankhulo kukambirana, 30-day KV page cache, daily housekeeping cron — ndipo sichifunikira kudziwa momwe deta idapangidwira.

**Mtengo:** D1 schema change imakhudza mafayilo awiri (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Inshuwalansi yotsika mtengo.

### Zinthu zosavomerezeka zomwe zakhazikika muzochita

- Siyigwirizana ndi boma la U.S.; palibe zizindikiro zovomerezeka.
- Zosasinthika zoyambira zimasungidwa, sizimasinthidwa.
- Kanema wochokera ku DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` pa webusaitiyi — yosakika ndi ma injini osakira, osalowa mu AI-scrape.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
