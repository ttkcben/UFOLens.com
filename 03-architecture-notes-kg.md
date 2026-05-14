# GitHub — Nsangu 3 ya 3 · Mambu ya Kutunga (Diskusio ya mutindu ya ADR)

**Sadila bonso:** Diskusio na nsi ya "Songa mpi Tanga" / "Kutunga", to kisina ya ADR ya `docs/`.
**Mvovo ya mfunu:** kutunga, ADR, masini ya ke kwendaka kaka na ntwala, LLM ya kisika, Ollama, OCR, edge computing, CSP, ba en-têtes ya securite, pipeline ya data, kuyidika bima ya kufuta, manifeste ya SQLite, D1, R2, KV
**Bikwati:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Sambu na nki bo me tungaka ufolens.com mutindu yai

Mambu ya me sonama na mambu tatu ya me tungaka [ufolens.com](https://www.ufolens.com) (kuvutula ya kusosa, ya bandinga mingi ya [arsiv ya PURSUE UAP](https://www.war.gov/ufo)). Mambu ya kutuba / ya kukangisa kele ya mbote.

### 1. Pipeline kele masini ya ke kwendaka kaka na ntwala — na luzolo

Bima ya kele: `me monana → me telecharger → ocr_done → me balula → me basisa`. Nkanda ke kwendaka kaka na ntwala, mpi kaka kana kisalu kele. Bima ya bo me basisa ke vutukaka ve na kusala yo diaka kana muntu ya ke monaka bansoba me mona ve nde kisina me sobaka.

**Sambu na nki:** OCR + nbalula kele bisalu ya ke lombaka mbongo mingi, mpi arsiv ke yelaka ti ntangu. Pipeline yina "ke vutukaka kusala bima yonso sambu na kukinga" kele ti ntalu ya me fula. Kukangisa kuvutuka na nima ke kangisaka mpi mbongo mingi ya kufuta. Ndilu ya ntalu kele diambu ya graphe ya bima, kansi ve ya meso ya muntu ya ke salaka.

**Ntalu:** kuvutula ba schema mpi kuvutula na kusala na luzolo kele ya mpasi na luzolo. Yo kele diambu ya mbote ya kupesa.

### 2. OCR mpi nbalula ke salamaka na LLM ya kisika, kansi ve na API ya matata

OCR: motere ya me fionguna, Tesseract CLI ya kusadila kana ya ntete kele ve. Nbalula + NER: Gemma na nzila ya Ollama, na ordinatere ya Apple Silicon.

**Sambu na nki:** nge ke futaka ve ata kima mosi sambu na konso nkanda; yo kele ya kuvutula (modèle ya me yidimaka + mambu ya kulomba); mpi kitini ya kubonga fwete salama na IP ya nzo (kisina kele na nima ya Akamai Bot Manager — `curl` ke bakaka 403), kansi ordinatere kele kaka na kati.

**Ntalu:** bondeko ya nbalula kele na nsi ya modèle ya nene. Sambu na arsiv ya kumeka yina Kingelesi ya kisina kele kaka na klik mosi, yo kele mbote. Beto ke tubaka ve nde nbalula kele ya luyalu.

### 3. Bitini zole ke kabulaka kaka interface mosi: kitini ya bo me basisa

Pipeline ke sonikaka ve na base de données ya kubasisa na mbala mosi. Yo ke basisaka `{ SQL, manifeste ya bima, lisiti ya kufimpasa cache }`. "Kubasisa" = kusadila kitini yina (kutinda SQL na base de données SQL ya nsongi, kuyidika bima na kisika ya kubumba bima, kufimpasa ba nsabi ya cache ya bo me tanga).

**Sambu na nki:** kitini ya kisika mpi kitini ya nsongi lenda yela na kwaya; kitini kele ya kutala; mpi "kutula data" kele ti kifwani mosi ntangu yonso. Worker kele programe ya fioti ya TypeScript/Hono — strict CSP (ata `unsafe-inline` ve; JSON-LD ya kati ya kanda kele ya me kangama na sha256), `Accept-Language` + nkubu ya bansi→ndinga, cache ya KV ya bilumbu 30, cron ya kuyidika bima konso kilumbu — mpi yo kele ve na mfunu ya kuzaba nki mutindu bo me salaka data.

**Ntalu:** nsasani ya schema ya D1 ke simba ba fisie zole (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Nsinsani ya ntalu fioti.

### Mambu ya ke sobaka ve na kimuntu

- Kele ve ya kuwakana ti luyalu ya États-Unis; ata bidimbu ya luyalu ve.
- Mikanda ya bo me fukisa ke bumbamaka, bo ke vutulaka yo ve.
- Video kele ya DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na site yonso — ya kusosa, me buya AI kubaka yo.

Na ntoto: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
