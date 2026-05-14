# GitHub — Post 1 da 3 · Annunzia da release / Bloc d'annunzia da README

**Utilisar sco:** in corp da release da GitHub, ina discussiun fixada, u l'entschatta dal README dal repo.
**Pleds-clav:** UAP, UFO, archiv PURSUE, documents declassifitgads, datas avertas, retschertga da text cumplaina, OCR, translaziun automatiasada, LLM local, Ollama, edge computing, API publica, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — ina plattafurma plurilingua e tschertgabla per l'archiv PURSUE UAP

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Archiv da funtauna:** https://www.war.gov/ufo

`ufolens.com` re-publitgescha l'archiv **PURSUE** dal Departament da Guerra dals Stadis Unids cun documents declassifitgads davart UAP / UFO sco plattafurma da savida: retschertga da text cumplaina, translaziun automatiasada tras l'entir corpus, exploraziun sin charta + cronologia, ed in'API publica da JSON. Ils documents da funtauna èn lavurs da la regenza federala dals Stadis Unids e tutgan en ils Stadis Unids al domini public ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Quest project **n'è betg affilià cun la regenza dals Stadis Unids**, n'utilisescha nagins insigns uffizials e na revochescha mai redacziuns.

### Architectura

```
Maschina locala (Apple Silicon, IP residenzial)       Rait da l'edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, cor be cun stdlib)          worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (be enavant)       /{lang}/...   paginas
  OCR: engine open-source (fallback sin Tesseract CLI) /api/v1/...   API publica
  translate / NER: LLM local (Gemma via Ollama)        /admin        consola da l'operatur
  stadi: manifest da SQLite                          sustegnì da: DB SQL da l'edge, memoria
        │                                              d'objects (PDFs da funtauna), cache KV
        └── publitgescha in pachet: SQL + manifest d'assets + glista da purgar la cache ──┘
```

- **Nagins custs da cloud-AI per document.** OCR e translaziun vegnan exequids localmain; la maschina da stadi che va be enavant (`scuvert → telechargià → ocr_fatg → translatà → publitgà`) garantescha ch'in document na vegn mai pli elavurà, nun che el è sa midà.
- **Il cor da la pipeline n'ha naginas dependenzas da terzs** — ils moduls da parsing / manifest / delta funcziuneschan e testan sin in Python net senza insatge installà cun pip; las etappas d'OCR/translaziun sa cumportan cun grazia sch'ils pachets opziunals mancan.
- **La pagina da l'edge** applichescha headers da segirezza + CSP severs (nagin `unsafe-inline`; JSON-LD inline cun sha256-fixà), negoziaziun da lingua via `Accept-Language` + mapping da pajais, in cache da pagina da 30 dis en KV, ed in cron da mantegniment da mintga di.
- **Agiuntaments incrementals:** in detectur da delta cumparescha l'index da funtauna e maina be midadas enavos en la pipeline.

### Per sviluppaders

L'API publica sin https://www.ufolens.com/api/v1 returna documents e metadatas sco JSON. L'access anonim è limità en la rata; dumandai ina clav per nivels da retschertga/sviluppader. Guardai la secziun da l'API sin la pagina per endpoints e limits.

### Status

Cudesch cumplet; pagina lantschada sin https://www.ufolens.com. La banca da datas da producziun vegn emplenida cun exequir la pipeline offline e publitgar il pachet enavant (`cli_publish run --remote`). La documentaziun da design cumplaina sa chatta en `docs/20260511/`.

### Licenza / cunfins

- Documents da funtauna: lavurs da la regenza federala dals Stadis Unids, domini public entaifer ils Stadis Unids.
- Il code agen da questa plattafurma: guardai `LICENSE`.
- La pagina trametta `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — indexabla da maschinas da tschertga, opt-out per training/scraping d'AI.
- Material video è attribuì a DVIDS / AARO e n'è betg pretendì da quest project.

Problems e PRs èn bainvegnids. Legi `CLAUDE.md` e `docs/20260511/00-*` avant che avrir midadas structuralas.

