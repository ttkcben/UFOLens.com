# GitHub — Articulu 1 di 3 · Annunziu di a versione / bloccu README

**Aduprà cum'è:** corpu di una Release GitHub, una Discussione appiccicata, o a cima di u README di u repositoriu.
**Parolle chjave:** UAP, UFO, archiviu PURSUE, ducumenti declassificati, dati aperti, ricerca testuale cumpleta, OCR, traduzzione automatica, LLM lucale, Ollama, edge computing, API publica, Hono, TypeScript, Python
**Ligami ipertestuali:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — una piattaforma multilingua è ricircabile per l'archiviu PURSUE UAP

**In diretta:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archiviu surghjente:** https://www.war.gov/ufo

`ufolens.com` ripubblica l'archiviu **PURSUE** di u Dipartimentu di a Guerra di i Stati Uniti di registrazioni declassificate di UAP / UFO cum'è una piattaforma di cunniscenza: ricerca testuale cumpleta, traduzzione automatica à traversu u corpus, esplorazione di carte + cronologia, è una API JSON publica. I ducumenti surghjente sò opere di u guvernu federale di i Stati Uniti è in i Stati Uniti sò di duminiu publicu ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Stu prugettu **ùn hè micca affiliatu cù u guvernu di i Stati Uniti**, ùn usa alcuna insignia ufficiale, è ùn inverte mai e redazzioni.

### Architettura

```
Macchina lucale (Apple Silicon, IP residenziale)     Reta edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, core stdlib-only)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pagine
  OCR: mutore open-source (fallback Tesseract CLI)     /api/v1/...   API publica
  translate / NER: LLM lucale (Gemma via Ollama)       /admin        cunsola di l'operatore
  statu: manifestu SQLite                             suppurtatu da: DB SQL edge, almacenamentu
        │                                              d'ogetti (PDF surghjente), cache KV
        └── publica un pacchettu: SQL + manifestu d'assi + lista di purga di cache ──┘
```

- **Costu zero per ducumentu in u cloud-AI.** L'OCR è a traduzzione sò eseguiti in lucale; a macchina à stati forward-only (`discovered → downloaded → ocr_done → translated → published`) garantisce chì nisun ducumentu ùn sia ritrattatu, salvu ch'ellu sia cambiatu.
- **U core di u pipeline ùn hà nisuna dipendenza di terze parti** — i moduli di parsing / manifestu / delta funzionanu è si testanu nantu à un Python pulitu senza nunda installatu via pip; e tappe di OCR/traduzzione si degradanu cun grazia quandu i pacchetti opzionali sò assenti.
- **U situ edge** applica intestazioni di securità strette + CSP (senza `unsafe-inline`; u JSON-LD in linea hè fissatu cù sha256), negoziazione di lingua via `Accept-Language` + cartugrafia di paese, una cache di pagina KV di 30 ghjorni, è un cron di mantenimentu cutidianu.
- **Aghjurnamenti incrementali:** un rilevatore di delta face a differenza di l'indici surghjente è furnisce solu i cambiamenti à u pipeline.

### Per i sviluppatori

L'API publica à https://www.ufolens.com/api/v1 restituisce ducumenti è metadati in formatu JSON. L'accessu anonimu hè limitatu in freccia; dumandate una chjave per i livelli di ricerca/sviluppatore. Vede a sezione API nantu à u situ per i punti d'accessu è i limiti.

### Statu

Codice cumpletu; situ sviluppatu à https://www.ufolens.com. A basa di dati di produzzione hè pupulata esecutendu u pipeline offline è publichendu u pacchettu in avanti (`cli_publish run --remote`). A ducumentazione cumpleta di u cuncepimentu si trova in `docs/20260511/`.

### Licenza / cunfini

- Ducumenti surghjente: opere di u guvernu federale di i Stati Uniti, duminiu publicu in i Stati Uniti.
- U codice propiu di sta piattaforma: vede `LICENSE`.
- U situ manda `Tdm-Reservation: 1` è `X-Robots-Tag: noai, noimageai` — indicizzabile da i mutori di ricerca, disattivatu per l'addestramentu/scraping da l'IA.
- E sequenze video sò attribuite à DVIDS / AARO è ùn sò micca rivendicate da stu prugettu.

I prublemi è i PR sò benvenuti. Per piacè, leghjite `CLAUDE.md` è `docs/20260511/00-*` prima di prupone cambiamenti strutturali.
