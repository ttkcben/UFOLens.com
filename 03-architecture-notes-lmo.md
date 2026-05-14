# GitHub — Post 3 de 3 · Aponcc de architetura (Discussion in stil ADR)

**Doperà 'me:** una Discussion sota "Fà vedé e cuntà su" / "Architetura", o 'me sbozz per un ADR in `docs/`.
**Paròle ciav:** architetura, ADR, machina a stacc che la va domà inanz, LLM local, Ollama, OCR, edge computing, CSP, header de segurezza, data pipeline, ingegneria di coscc, manifest SQLite, D1, R2, KV
**Colegamencc ipertestuai:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Perchè ufolens.com l'è fad su in 'sta manera

Aponcc in sui trè decision che i hann modellad [ufolens.com](https://www.ufolens.com) (la ricostruzzion con fonzion de rezercha e multilengh de l'archivi [PURSUE UAP](https://www.war.gov/ufo)). Comencc / obbiezzion benvegnude.

### 1. La pipeline l'è una machina a stacc che la va domà inanz — e l'è volud

Stacc: `descovert → descaregad → ocr_fad → tradot → publicad`. Un document el va domà inanz, e domà quand che gh'è de lavorà. El contegnud publicad l'è mai ri-elaborad a men che un rilevator de delta el veda che la fœn l'è cambiada devera.

**Perchè:** OCR + traduzzion i enn i operazzion costose, e l'archivi el cress in del temp. Una pipeline che la "fa tornà a partì tut per segurezza" la gh'ha un cost senza fin. Riusì no a tornà indree el rend impossibel un cont che el scapa de man. El tecc di coscc l'è una proprietà del graf di stacc, no de la vigilanza de l'operator.

**Cost:** i migrazzion del schema e la ri-elaborazzion fada aposta i enn scomode de intent. Un compromess che se pœl acetà.

### 2. L'OCR e la traduzzion i vann con un LLM local, no con una API in sul cloud

OCR: motor open-source, Tesseract CLI 'me fallback. Traduzzion + NER: Gemma via Ollama, in su un laptop Apple Silicon.

**Perchè:** cost marginal zero per document; reprodusibel (modell e prompt fiss); e la fas de aquisizzion la gh'ha giamò de andà de un IP residenzial (la fœn l'è dedree a Akamai Bot Manager — `curl` el ciapa un 403), donca un laptop l'è de tœucc i mœud in del mes.

**Cost:** la qualità de la traduzzion l'è pussee bassa de quella de un modell de frontiera. Per un corpus de referenza indove l'original in ingles l'è semper a un clic de distanza, va ben istess. Diem no che i traduzzion i enn autorevoli.

### 3. I do part i gh'hann in comun domà una interfacia: un pachet publicad

La pipeline la scriv mai diretament in del database de produzzion. La buta fœra `{ SQL, manifest di asset, lista de netà la cache }`. "Publegà" = aplicà 'sto pachet inanz (mandà l'SQL al DB SQL de l'edge, sincronizà i asset in sul stocagg ogecc, netà i ciav de cache nomade).

**Perchè:** la part local e la part edge i pœden evolver in manera independenta; el pachet el se pœl revardà; e "distribuì i dacc" l'è semper istess. El Worker l'è una piscinina aplicazzion in TypeScript/Hono — CSP sever (nissun `unsafe-inline`; el JSON-LD in linia l'è fissad con sha256), negoziazzion `Accept-Language` + paes→lengua, cache de pagina KV de 30 dì, cron de netada giornalier — e el gh'ha mai besogn de savé 'me che i dacc i enn stacc fad.

**Cost:** un cambiament al schema de D1 el toca du file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un'assicurazzion che la costa poch.

### Robe no negoziabij implementade in del comportament

- Minga sociad con el govern di Stacc Unicc; nissuna insegna ofizzial.
- I test scondœucc di fœn i resten e i enn mai cavacc via.
- I filmacc i enn atribuid a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` in tut el sit — se pœl indexà per la rezercha, ma l'è tirad fœra del scraping di AI.

In diretta: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

