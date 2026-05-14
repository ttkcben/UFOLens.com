# GitHub — Post 3 di 3 · Note sull'architettura (Discussione in stile ADR)

**Uso:** una Discussione in "Show and tell" / "Architecture", o come base per un ADR in `docs/`.
**Parole chiave:** architettura, ADR, macchina a stati forward-only, LLM locale, Ollama, OCR, edge computing, CSP, header di sicurezza, pipeline di dati, ingegneria dei costi, manifest SQLite, D1, R2, KV
**Hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Perché ufolens.com è costruito in questo modo

Note sulle tre decisioni che hanno plasmato [ufolens.com](https://www.ufolens.com) (la ricostruzione ricercabile e multilingue dell'[archivio PURSUE UAP](https://www.war.gov/ufo)). Commenti e critiche costruttive sono benvenuti.

### 1. La pipeline è una macchina a stati a solo avanzamento — di proposito

Stati: `discovered → downloaded → ocr_done → translated → published`. Un documento si muove solo in avanti, e solo quando c'è del lavoro da fare. Il contenuto pubblicato non viene mai rielaborato a meno che un rilevatore di delta non veda che la fonte è effettivamente cambiata.

**Perché:** OCR + traduzione sono le operazioni costose e l'archivio cresce nel tempo. Una pipeline che "riesegue tutto per sicurezza" ha un costo illimitato. Rendere impossibili le transizioni all'indietro rende impossibile avere costi fuori controllo. Il tetto di costo è una proprietà del grafo degli stati, non della vigilanza dell'operatore.

**Costo:** le migrazioni dello schema e la rielaborazione intenzionale sono deliberatamente scomode. Un compromesso accettabile.

### 2. OCR e traduzione girano su un LLM locale, non su un'API cloud

OCR: motore open-source, con fallback su Tesseract CLI. Traduzione + NER: Gemma tramite Ollama, su un laptop Apple Silicon.

**Perché:** costo marginale zero per documento; riproducibilità (modello + prompt fissi); e la fase di fetch deve già essere eseguita da un IP residenziale (la fonte è dietro Akamai Bot Manager — `curl` riceve un 403), quindi un laptop è comunque coinvolto.

**Costo:** la qualità della traduzione è inferiore a quella di un modello di frontiera. Per un corpus di riferimento in cui l'originale inglese è sempre a un clic di distanza, va bene. Non affermiamo che le traduzioni siano autorevoli.

### 3. Le due metà condividono esattamente un'interfaccia: un bundle pubblicato

La pipeline non scrive mai direttamente nel database di produzione. Emette un `{ SQL, manifest degli asset, lista di cache da invalidare }`. "Pubblicare" = applicare questo bundle (eseguire l'SQL sul DB SQL edge, sincronizzare gli asset sull'object storage, invalidare le chiavi di cache specificate).

**Perché:** il lato locale e il lato edge possono evolvere in modo indipendente; il bundle è revisionabile; e il "deploy dei dati" ha sempre la stessa forma. Il Worker è una piccola app TypeScript/Hono — CSP restrittiva (no `unsafe-inline`; JSON-LD inline con pinning sha256), negoziazione `Accept-Language` + paese→lingua, cache di pagina KV di 30 giorni, cron di manutenzione giornaliero — e non ha mai bisogno di sapere come sono stati creati i dati.

**Costo:** una modifica allo schema D1 interessa due file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un'assicurazione a basso costo.

### Principi non negoziabili integrati nel comportamento

- Non affiliato con il governo degli Stati Uniti; nessuna insegna ufficiale.
- Le parti redatte nei documenti originali sono preservate, mai annullate.
- Video attribuito a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` su tutto il sito — indicizzabile per la ricerca, ma escluso dallo scraping delle IA.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
