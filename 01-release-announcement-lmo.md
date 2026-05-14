# GitHub — Post 1 de 3 · Anœunci de Release / Blòch per el README

**Doperà 'me:** còrp de una Release de GitHub, una Discussion fissada, o in scima al README del repository.
**Paròle ciav:** UAP, UFO, archivi PURSUE, documencc desclassificacc, dacc avercc, rezercha testual completa, OCR, traduzzion automatica, LLM local, Ollama, edge computing, API publica, Hono, TypeScript, Python
**Colegamencc ipertestuai:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — una piataforma multilengh e con fonzion de rezercha per l'archivi PURSUE UAP

**In diretta:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archivi di fœn:** https://www.war.gov/ufo

`ufolens.com` el publica de nœuv l'archivi **PURSUE** del Dipartiment de la Guerra di Stacc Unicc con dent i documencc desclassificacc UAP / UFO 'me piataforma de cognossenza: rezercha testual completa, traduzzion automatica in su tut el corpus, esplorazzion de mapa e linea del temp, e una API publica JSON. I documencc originai i enn œvre del govern federal di Stacc Unicc e, in di Stacc Unicc, i enn de domini publich ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). 'Sto projet l'è **minga sociad con el govern di Stacc Unicc**, el dopera nissuna insegna ofizzial, e el cava mai via i test scondœucc.

### Architetura

```
Machina local (Apple Silicon, IP residenzial)         Rede edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, còrp domà stdlib)            worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (domà inanz)      /{lang}/...   pagine
  OCR: motor open-source (Tesseract CLI 'me fallback)   /api/v1/...   API publica
  translate / NER: LLM local (Gemma via Ollama)        /admin        console de l'operator
  stat: manifest SQLite                              sostegnœucc da: DB SQL edge,
        │                                              stocagg ogecc (PDF di fœn), cache KV
        └── el publica un pachet: SQL + manifest di asset + lista de netà la cache ──┘
```

- **Costo zero de cloud-AI per document.** OCR e traduzzion i vann in local; la machina a stacc che la va domà inanz (`descovert → descaregad → ocr_fad → tradot → publicad`) la garantiss che nissun document el vegna ri-elaborad se l'è no cambiad.
- **El còrp de la pipeline el gh'ha nissuna dipendenza de terz part** — i modui de parsing / manifest / delta i vann e i enn testacc con un Python net senza nient de instalad con pip; i fasi de OCR/traduzzion i se degraden con grazia quand che i pachecc opzionai i gh'enn no.
- **El sit Edge** el dopera di header de segurezza sever e CSP (nissun `unsafe-inline`; el JSON-LD in linia l'è fissad con sha256), negoziazzion de la lengua a travers de `Accept-Language` + mapadura del paes, una cache de pagina KV de 30 dì, e un cron de netada giornalier.
- **Insement adasi adasi:** un rilevator de delta el confronta l'index di fœn e el manda domà i cambiamencc indree in de la pipeline.

### Per i desvilupador

L'API publica a https://www.ufolens.com/api/v1 la da indree documencc e metadacc 'me JSON. L'acess anonim l'è limitad in de la sveltezza; domanda una ciav per i livei de ricercador/desvilupador. Varda la sezzion API in sul sit per i endpoint e i limicc.

### Stat

Còdes completad; sit publicad a https://www.ufolens.com. El database de produzzion l'è popolad con l'esecuzzion de la pipeline fœra linia e la publicazzion del pachet inanz (`cli_publish run --remote`). I documencc de progetazzion complet i se trœven in `docs/20260511/`.

### Lizenza / confin

- Documencc originai: œvre del govern federal di Stacc Unicc, de domini publich in di Stacc Unicc.
- El còdes de 'sta piataforma: varda `LICENSE`.
- El sit el manda `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — se pœden indexà di motor de rezercha, ma l'è tirad fœra de l'adestrament/scraping di AI.
- I filmacc i enn atribuid a DVIDS / AARO e i enn no reclamacc de 'sto projet.

Segnalazzion de problema e PR benvegnude. Per piasé, lensg `CLAUDE.md` e `docs/20260511/00-*` prima de dervì cambiamencc struturai.

