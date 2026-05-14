# GitHub — Icipande 3 pali 3 · Ifyalandwe fya architecture (ADR-style Discussion)

**Cibomfiwe nga:** Ifyalandwe fyatekelwe pasi pa "Show and tell" / "Architecture", nelyo `docs/` ADR seed.
**Amashiwi akulu:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mulandu nshi ufolens.com yapangilwila ifyo yaba

Ifya kulemba pa fishinte fitatu ifyafulishe [ufolens.com](https://www.ufolens.com) (ukutungulula, ukwafwaya ifyakufwaya, ukwabela indimi ishingi ku [PURSUE UAP archive](https://www.war.gov/ufo)). Ifyalandwe / ukukana kwapokelelwa.

### 1. Pipeline ili forward-only state machine — mu mulandu

States: `discovered → downloaded → ocr_done → translated → published`. Document ipita fye pa ntanshi, kabili fye nga kuli umulimo wa kubomba. Content iyaishibwa tayakabweshiwe nakabili kano delta detector ilamona ukuti source yalikwetepo ifyacinchika.

**Mulandu nshi:** OCR + ukupilibula e milimo iyakalipa, kabili archive ilakula pa nshita. Pipeline iyakubomba "ukulenga fyonse ukwaba bwino" yakwata cost iyakale. Ukulenga ukuti takwaba ifyacinchika ifya ku muyanda kulenga ukuti takwaba bill iyakale. Cost ceiling ili property ya state graph, te ku kulanguluka kwa operator.

**Cost:** schema migrations na reprocessing-on-purpose fyacitika mu mulandu. Tradeoff iyapokelelwa.

### 2. OCR na ukupilibula fipita pa local LLM, te pa cloud API

OCR: open-source engine, Tesseract CLI fallback. Ukupilibula + NER: Gemma ukubomfya Ollama, pa Apple Silicon laptop.

**Mulandu nshi:** zero marginal cost pa document; reproducible (fixed model + prompts); kabili fetch step ili kale na ukubomba pa residential IP (source ili pasi pa Akamai Bot Manager — `curl` ikasanga 403), nso laptop ili mu loop.

**Cost:** translation quality ili pasi pa frontier model. Ku reference corpus apo original English ili fye one click away, ifi fwafikeme. Tatulelanda ukuti ukupilibula kwetu kwacindama.

### 3. Ifipande fibili filabomba pa interface imo fye: a published bundle

Pipeline tayakalemba pa production database mu ncende. Ilalenga `{ SQL, asset manifest, cache-purge list }`. "Ukubilisha" = ukubomfya bundle pa ntanshi (ukutuma SQL ku edge SQL DB, ukulumbanya assets ku object storage, ukucekela cache keys ishaishibwa).

**Mulandu nshi:** local side na edge side fingacinchika mu mucalo; bundle ila reviewable; kabili "deploy data" ili the same shape pa nshita yonse. Worker ili small TypeScript/Hono app — strict CSP (takwaba `unsafe-inline`; inline JSON-LD ili sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — kabili tayafwaya ukwishiba ifyo data yapangiwe.

**Cost:** a D1 schema change ifibomba pa mafunde yabili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Cheap insurance.

### Ifishinte ifyaishibwa mu myendele

- Takwaba filubo ne U.S. government; takwaba isonko lyakubilisha.
- Source redactions shilabika, tayapilibulwa.
- Video yatumbikwa ku DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` pa site yonse — search-indexable, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
