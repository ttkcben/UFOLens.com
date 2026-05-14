# GitHub — Publicacion 3 de 3 · Notas de architectura (Discussion de stilo ADR)

**Usar como:** un Discussion sub "Presentation e demonstration" / "Architectura", o como un base pro ADR in `docs/`.
**Parolas clave:** architectura, ADR, machina de statos a progression solmente, LLM local, Ollama, OCR, computation al bordo, CSP, capites de securitate, pipeline de datos, ingenieria de custos, manifesto SQLite, D1, R2, KV
**Hyperligamines:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Proque ufolens.com es construite de iste maniera

Notas sur le tres decisiones que ha plasmate [ufolens.com](https://www.ufolens.com) (le reconstruction perquisibile e multilingue del [archivo PURSUE UAP](https://www.war.gov/ufo)). Commentarios / refutationes es benvenite.

### 1. Le pipeline es un machina de statos a progression solmente — con intention

Statos: `discovered → downloaded → ocr_done → translated → published`. Un documento solmente avanta, e solmente quando il ha labor a facer. Contenuto publicate non es unquam reprocessate, a minus que un detector de deltas vide que le fonte ha cambiate realmente.

**Proque:** OCR + traduction es le operationes costose, e le archivo cresce con le tempore. Un pipeline que "re-executa tote pro esser secur" ha un costo illimitate. Render impossibile le transitiones a retro rende impossibile un factura exorbitante. Le plafon de costo es un proprietate del grapho de statos, non del vigilantia del operator.

**Costo:** migrationes de schema e reprocessamento intential es deliberatemente incommodante. Un compromisso acceptabile.

### 2. OCR e traduction se executa sur un LLM local, non un API del nube

OCR: motor de codice aperte, con Tesseract CLI como alternativa. Traduction + NER: Gemma via Ollama, sur un laptop Apple Silicon.

**Proque:** costo marginal zero per documento; reproducibile (modello e instructiones fixe); e le passo de recuperation ja debe esser executate desde un IP residential (le fonte es detra Akamai Bot Manager — `curl` recipe un 403), dunque un laptop es jam in le circuito de tote maniera.

**Costo:** le qualitate del traduction es inferior a un modello de frontiera. Pro un corpus de referentia ubi le original anglese es sempre a un clic de distantia, isto es acceptabile. Nos non pretende que le traductiones es authoritative.

### 3. Le duo medietates condivide exactemente un interfacie: un pacchetto publicate

Le pipeline nunquam scribe directemente in le base de datos de production. Illo emitte `{ SQL, manifesto de activos, lista de purga de cache }`. "Publicar" = applicar ille pacchetto in avante (mitter le SQL al base de datos SQL del bordo, synchronisar le activos al immagazinage de objectos, purgar le claves de cache nominate).

**Proque:** le latere local e le latere del bordo pote evoluer independentemente; le pacchetto es revisabile; e "displicar datos" ha sempre le mesme forma. Le Worker es un parve app de TypeScript/Hono — CSP stricte (non `unsafe-inline`; `inline JSON-LD` es affixate con `sha256`), negotiation `Accept-Language` + pais→lingua, cache de pagina KV de 30 dies, cron de mantenentia diari — e illo nunquam necessita saper como le datos esseva create.

**Costo:** un cambio de schema in D1 tocca duo files (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un assecuration bon mercato.

### Principios non negotiabile integrate in le comportamento

- Non affiliate con le governamento del S.U.; nulle insignias official.
- Le redactiones del fonte es preservate, nunquam revertite.
- Video attribuite a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` a transverso del sito — indexabile pro recerca, optate foras del scraping per AI.

In directo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
