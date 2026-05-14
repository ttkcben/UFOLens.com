# GitHub — Articulu 3 di 3 · Note d'architettura (Discussione in stile ADR)

**Aduprà cum'è:** una Discussione sottu "Mostra è conta" / "Architettura", o una basa per un ADR in `docs/`.
**Parolle chjave:** architettura, ADR, macchina à stati forward-only, LLM lucale, Ollama, OCR, edge computing, CSP, intestazioni di securità, pipeline di dati, ingegneria di i costi, manifestu SQLite, D1, R2, KV
**Ligami ipertestuali:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Perchè ufolens.com hè custruitu cum'ellu hè

Note nantu à e trè decisioni chì anu furmatu [ufolens.com](https://www.ufolens.com) (a ricustruzzione ricircabile è multilingua di l'[archiviu PURSUE UAP](https://www.war.gov/ufo)). Cumenti / critiche sò benvenuti.

### 1. U pipeline hè una macchina à stati forward-only — apposta

Stati: `discovered → downloaded → ocr_done → translated → published`. Un ducumentu si move solu in avanti, è solu quandu ci hè travagliu da fà. U cuntenutu publicatu ùn hè mai ritrattatu, salvu chì un rilevatore di delta veda chì a surghjente hà effettivamente cambiatu.

**Perchè:** l'OCR + a traduzzione sò l'uperazioni costose, è l'archiviu cresce cù u tempu. Un pipeline chì "riesegue tuttu per sicurezza" hà un costu illimitatu. Rende e transizioni in daretu impussibili rende una fattura fora di cuntrollu impussibile. U tettu di i costi hè una pruprietà di u graficu di stati, micca di a vigilanza di l'operatore.

**Costu:** e migrazioni di schema è u ritrattamentu apposta sò deliberatamente goffi. Un cumprumessu accettabile.

### 2. L'OCR è a traduzzione funzionanu nantu à un LLM lucale, micca una API cloud

OCR: mutore open-source, fallback Tesseract CLI. Traduzzione + NER: Gemma via Ollama, nantu à un laptop Apple Silicon.

**Perchè:** costu marginale zero per ducumentu; riproducibile (mudellu + prompt fissi); è a tappa di recuperu deve dighjà esse eseguita da un IP residenziale (a surghjente hè daretu à Akamai Bot Manager — `curl` riceve un 403), dunque un laptop hè in ogni casu in u ciclu.

**Costu:** a qualità di a traduzzione hè inferiore à un mudellu di fruntiera. Per un corpus di riferenza induve l'uriginale inglese hè sempre à un clic di distanza, va bè. Ùn pretendemu micca chì e traduzzioni sianu authoritative.

### 3. E duie metà spartenu esattamente una interfaccia: un pacchettu publicatu

U pipeline ùn scrive mai direttamente in a basa di dati di produzzione. Emette `{ SQL, manifestu d'assi, lista di purga di cache }`. "Publicà" = applicà quellu pacchettu in avanti (spinghje SQL à a basa di dati SQL edge, sincronizà l'assi cù l'almacenamentu d'ogetti, purgà e chjave di cache nominate).

**Perchè:** a parte lucale è a parte edge ponu evoluzione indipindente; u pacchettu hè rivedibile; è "sviluppà dati" hà a stessa forma ogni volta. U Worker hè una piccula app TypeScript/Hono — CSP strettu (senza `unsafe-inline`; u JSON-LD in linea hè fissatu cù sha256), negoziazione `Accept-Language` + paese→lingua, cache di pagina KV di 30 ghjorni, cron di mantenimentu cutidianu — è ùn hà mai bisognu di sapè cumu i dati sò stati creati.

**Costu:** un cambiamentu di schema D1 tocca dui schedarii (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un'assicuranza à bon pattu.

### Non negoziabili integrati in u cumpurtamentu

- Micca affiliatu cù u guvernu di i Stati Uniti; nisuna insignia ufficiale.
- E redazzioni surghjente sò priservate, mai invertite.
- Video attribuitu à DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` in tuttu u situ — indicizzabile da i mutori di ricerca, disattivatu per u scraping da l'IA.

In diretta: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
