# GitHub — Post 3 ’e 3 · Note ’e architettura (Discussione stile ADR)

**Ausà comme:** na Discussione sotto "Mostra e conta" / "Architettura", o comme semenza p’ ’a `docs/` ADR.
**Parole chiave:** architettura, ADR, machina a state ca va solo ’nnante, LLM locale, Ollama, OCR, edge computing, CSP, ’ntestazione ’e sicurezza, data pipeline, ingegneria d’ ’o costo, manefesto SQLite, D1, R2, KV
**Culligamente:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Pecché ufolens.com è custruito accussì

Note ncopp’ ’e tre decisione ca hanno dato forma a [ufolens.com](https://www.ufolens.com) (’a ricostruzzione cercabbile e multilingua d’ ’o [archivio PURSUE UAP](https://www.war.gov/ufo)). Cumente / critiche so’ benvenute.

### 1. ’O pipeline è na machina a state ca va solo ’nnante — apposta

State: `scuperto → scarrecato → ocr_fatto → tradutto → pubbrecato`. Nu documento se move solo ’nnante, e solo quanno ce sta fatica ’a fa. ’O cuntinuto pubbrecato nun ven’ maje ri-processato a meno ca nu rilevatore ’e delta vede ca ’a surgenta è cagnata overo.

**Pecché:** OCR + traduzzione so’ ll'operazzione cchiù custose, e ll'archivio cresce cu ’o tiempo. Nu pipeline ca "rifà tutto pe sicurezza" tene nu costo senza fine. Rènnere ’e transizzione a retuso ’mpussibbele fa addeventà ’mpussibbele na spesa ca scappa ’e mano. ’O tetto d’ ’o costo è na pruprietà d’ ’o grafo d’ ’e state, nun d’ ’a vigilanza ’e chi opera.

**Costo:** ’e migrazzione d’ ’o schema e ’a ri-processazzione apposta so’ volutamente scomode. Nu compromesso accettebbele.

### 2. OCR e traduzzione girano ncopp’a nu LLM locale, nun ncopp’a n’API cloud

OCR: motore open-source, fallback Tesseract CLI. Traduzzione + NER: Gemma via Ollama, ncopp’a nu laptop Apple Silicon.

**Pecché:** costo marginale zero pe documento; riproducibbele (modello fisso + prompt); e ’a fase ’e recupero già adda girà ’a n'IP residenziale (’a surgenta sta arreto a Akamai Bot Manager — `curl` piglia nu 403), quinni nu laptop sta già ’mmiezo ’a facenna.

**Costo:** ’a qualità d’ ’a traduzzione è cchiù vascia ’e nu modello ’e fruntiera. Pe nu corpus ’e riferimiento addó ll’inglese originale sta sempe a nu click ’e distanza, va buono. Nun dicimmo ca ’e traduzzione so’ autorevole.

### 3. ’E doje mità spartono esattamente n'interfaccia: nu pacchetto pubbrecato

’O pipeline nun scrive maje direttamiente ncopp’ ’o database ’e produzzione. Produce `{ SQL, manefesto d’ ’e asset, lista pe spurgo cache }`. "Pubbrecà" = appulecà chillo pacchetto ’nnante (spignere l'SQL ncopp’ ’o DB SQL edge, sincronizzà ll'asset ncopp’ ’o storage a uggette, spurgà ’e chiave d’ ’a cache nominate).

**Pecché:** ’a parte locale e ’a parte edge ponno evolvere ’ndipendentemente; ’o pacchetto se po’ revisionà; e "distribbuì date" tene sempe ’a stessa forma. ’O Worker è na piccerella app TypeScript/Hono — CSP rigido (nisciun `unsafe-inline`; ’o JSON-LD inline è appuntato cu sha256), negoziazzione `Accept-Language` + paese→lengua, cache d’ ’e paggene KV ’e 30 juorne, cron ’e pulizzia ’e tutte ’e juorne — e nun tene maje bisogno ’e sapé comme so’ state fatte ’e date.

**Costo:** nu cagnamiento d’ ’o schema D1 tocca doje file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). N'assicurazzione ca costa poco.

### ’E cose nun neguzziabbele ’ntrecciate ’int’ ’o comportamento

- Nun simmo affiliate cu ’o guvierno d’ ’e State Aunite; nisciun simmolo ufficiale.
- ’E ridazzione d’ ’a surgenta se cunservano, nun se cancellano maje.
- Video attribuito a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` pe tutto ’o sito — se po’ indicizzà p’ ’a ricerca, s’è tirato fore d’ ’o scraping pe ll'AI.

Dal vivo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

