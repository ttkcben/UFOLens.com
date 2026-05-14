# GitHub — Post 1 ’e 3 · Annuncio ’e Release / Blocco annuncio p’ ’o README

**Ausà comme:** nu corpo ’e na Release GitHub, na Discussione appuntata, o ’ncoppa ’o README d’ ’o repo.
**Parole chiave:** UAP, UFO, archivio PURSUE, ducumente declassificate, date aperte, ricerca full-text, OCR, traduzzione automatica, LLM locale, Ollama, edge computing, API pubbreca, Hono, TypeScript, Python
**Culligamente:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — na piattaforma multilingua e cercabbile pe ll'archivio PURSUE UAP

**Dal vivo:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archivio surgenta:** https://www.war.gov/ufo

`ufolens.com` ripubbreca ll'archivio **PURSUE** d’ ’o Dipartimento d’ ’a Guerra d’ ’e State Aunite cu ’e riggistre declassificate ncopp’a UAP / UFO comme na piattaforma ’e canuscenza: ricerca a testo completo, traduzzione automatica ’int’a tutto ’o corpus, splorazzione ncopp’a na mappa + linea d’ ’o tiempo, e n’API pubbreca JSON. ’E ducumente surgente so’ fatiche d’ ’o guvierno federale d’ ’e State Aunite e ’int’a ll’Amereca so’ de pubbreco dominio ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Stu pruggetto **nun è affiliato cu ’o guvierno d’ ’e State Aunite**, nun ausa simbole ufficiale e nun cancella maje ’e ridazzione.

### Architettura

```
Machina locale (Apple Silicon, IP residenziale)     Rete edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, core solo stdlib)           worker/  (TypeScript, Hono.js)
  piglià → OCR → traducere → pubbrecà  (solo ’nnante)    /{lang}/...   paggene
  OCR: motore open-source (fallback Tesseract CLI)     /api/v1/...   API pubbreca
  traducere / NER: LLM locale (Gemma via Ollama)        /admin        console operatore
  stato: manefesto SQLite                             supportato ’a: DB SQL edge,
        │                                              storage a uggette (PDF surgente), cache KV
        └── pubbreca nu pacchetto: SQL + manefesto asset + lista ’e spurgo cache ──┘
```

- **Nisciun costo cloud-AI p’ ogne documento.** OCR e traduzzione se fanno localmente; ’a machina a state ca va solo ’nnante (`scuperto → scarrecato → ocr_fatto → tradutto → pubbrecato`) garantisce ca nisciun documento ven’ "ri-processato" a meno ca nun è cagnato.
- **’O core d’ ’o pipeline nun tene dipendenze ’e terze parte** — ’e module pe ll'analisi / manefesto / delta funzionano e se provano ncopp’a nu Python pulito senza niente ’e ’nstallato cu pip; ’e fase ’e OCR/traduzzione degradano cu grazia quanno ’e pacchette opzionale nun ce stanno.
- **’O sito edge** appuleca ’ntestazione ’e sicurezza + CSP rigide (no `unsafe-inline`; ’o JSON-LD inline è appuntato cu sha256), negoziazzione d’ ’a lengua via `Accept-Language` + mappatura d’ ’o paese, na cache d’ ’e paggene KV ’e 30 juorne, e nu cron ’e pulizzia ’e tutte ’e juorne.
- **Aggiurnamente incrementale:** nu rilevatore ’e delta fa ’a differenza cu ll'indice surgenta e ’ntroduce solo ’e cagnamiente arrass’ ’o pipeline.

### Pe ’e sveluppature

L'API pubbreca a https://www.ufolens.com/api/v1 torna ducumente e metadata comme JSON. L'accesso anonimo è limitato; addimannate na chiave pe ’e livelle ’e cercatore/sveluppatore. Vedite ’a sezzione API ncopp’ ’o sito pe ll'endpoints e ’e limite.

### Stato

Codice completo; sito ’nstallato a https://www.ufolens.com. ’O database de produzzione ven’ carrecato facenno partì ’o pipeline offline e pubbrecanno ’o pacchetto ’nnante (`cli_publish run --remote`). ’E ducumente de design complete se trovano ’int’a `docs/20260511/`.

### Licenza / cunfine

- Ducumente surgente: fatiche d’ ’o guvierno federale d’ ’e State Aunite, de pubbreco dominio ’int’a ll’Amereca.
- ’O codice proprio de sta piattaforma: vedite `LICENSE`.
- ’O sito manna `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — se po’ indicizzà da ’e muture ’e ricerca, s’è tirato fore d’ ’o training/scraping ’e ll'AI.
- ’E filmate video so’ attribuite a DVIDS / AARO e nun so’ reclamate ’a stu pruggetto.

Issue e PR so’ benvenute. Pe piacere, leggete `CLAUDE.md` e `docs/20260511/00-*` primma d’aprire cagnamiente strutturale.

