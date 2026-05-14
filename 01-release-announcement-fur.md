# GitHub — Post 1 di 3 · Anunzi di version / Bloche anunzi README

**Ús come:** cuarp di une version di GitHub, une discussion fissade, o in cime al README dal repository.
**Perpaulis clâf:** UAP, UFO, archivi PURSUE, documents declassificâts, dâts vierts, ricercje a test complet, OCR, traduzion automatiche, LLM locâl, Ollama, edge computing, API publiche, Hono, TypeScript, Python
**Leams ipertestuâi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — une plateforme multilengâl e ricercjabil pal archivi PURSUE UAP

**Dal vîf:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archivi origjinâl:** https://www.war.gov/ufo

`ufolens.com` al torne a publicâ l'archivi **PURSUE** dal Dipartiment de Vuere dai Stâts Unîts sui documents declassificâts UAP / UFO come une plateforme di cognossince: ricercje a test complet, traduzion automatiche tra dut il corpus, esplorazion su mape e cronologjie, e une API publiche JSON. I documents origjinâi a son opares dal guvier federâl dai Stâts Unîts e, tai Stâts Unîts, a son di domini public ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Chest progjet **nol è afiliât cul guvier dai Stâts Unîts**, nol dopre insegnes uficiâls e nol gjave mai lis redazions.

### Architeture

```
Machine locâl (Apple Silicon, IP residenziâl)       Rêt edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, cûr dome stdlib)            worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (dome indevant)   /{lang}/...   pagjinis
  OCR: motôr open-source (fallback Tesseract CLI)      /api/v1/...   API publiche
  translate / NER: LLM locâl (Gemma vie Ollama)        /admin        console operadôr
  stât: manifest SQLite                              sostignût di: DB SQL edge, 
        │                                              memorie a ogjets (PDF origjinâi),
        └── al publiche un pachet: SQL + manifest asset + liste di svuedâ cache ──┘
```

- **Costi zero par document par AI sul cloud.** OCR e traduzion a funzionin in locâl; la machine a stâts dome indevant (`discovered → downloaded → ocr_done → translated → published`) e garantìs che nissun document nol vegni rielaborât, fûr che se al è cambiât.
- **Il cûr de pipeline nol à dependencis di tiercis parts** — i modui di parsing / manifest / delta a funzionin e a son testâts su un Python net cence nuie instalât vie pip; lis fasis di OCR/traduzion a degradin cun grâce cuant che i pachets opzionâi no son presints.
- **Il sît edge** al apliche intestazions di sigurece e CSP rigurosis (nissun `unsafe-inline`; JSON-LD in linie blocât cun sha256), negoziazion de lenghe vie `Accept-Language` + mapature par paîs, une cache de pagjine KV di 30 dîs e un cron di manutenzion gjornalîr.
- **Inzornaments incrementâi:** un rileva-diferencis al confronte l'indiç de sorzint e al imet te pipeline dome i cambiaments.

### Par i svilupadôrs

La API publiche su https://www.ufolens.com/api/v1 e furnìs documents e metadâts come JSON. L'acès anonim al à un limit di velocitât; domande une clâf par i nivei di ricercjadôr/svilupadôr. Cjale la sezion API sul sît par vê informazions sui endpoints e sui limits.

### Stât

Codis complet; sît implementât su https://www.ufolens.com. Il database di produzion al ven popolât fasint lâ la pipeline offline e publicant il pachet (`cli_publish run --remote`). I documents di progjet complets a son in `docs/20260511/`.

### Licence / limits

- Documents origjinâi: opares dal guvier federâl dai Stâts Unîts, di domini public tai Stâts Unîts.
- Il codis di cheste plateforme: cjale `LICENSE`.
- Il sît al mande `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — indiçizabil dai motôrs di ricercje, ma esclûs de adestrament/racuelte dâts pe AI.
- I filmâts a son atribuîts a DVIDS / AARO e no son rivendicâts di chest progjet.

Segnalazions e PRs a son benvignudis. Par plasê, lieç `CLAUDE.md` e `docs/20260511/00-*` prime di vierzi propuestis di cambiaments struturâi.

