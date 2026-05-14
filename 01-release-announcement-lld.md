# GitHub — Post 1 de 3 · Blòch de anunzie de Release / README

**Dovrar coche:** n corp de na Release de GitHub, na discussion fisseda o la pert auta del README del repo.
**Paroles clau:** UAP, UFO, archif PURSUE, documënc dessegredé, open data, inrescida a test plin, OCR, traduzion automatica, LLM local, Ollama, edge computing, API publica, Hono, TypeScript, Python
**Coleganc:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — na plataforma plurilingue e consultabla per l'archif PURSUE UAP

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Archif surant:** https://www.war.gov/ufo

`ufolens.com` republichea l'archif **PURSUE** del Departimënt de viera di Stac Unic de documënc dessegredé UAP / UFO coche na plataforma de savëi: inrescida a test plin, traduzion automatica tres dut l corpus, esplorazion de chertes y linies cronologiches, y na API publica JSON. I documënc surant ie lëures del guviern federal di Stac Unic y, te chëi stac, ie de domini publich ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Chësc proiet **ne ie nia afilià al guviern di Stac Unic**, no dovra degun segn ufizial y no revoca mei redazions.

### Architetura

```
Maschin local (Apple Silicon, IP residenzial)        Rë de bòrd (Edge network)
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, corp stdlib-only)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (ma inant)        /{lang}/...   plates
  OCR: motor open-source (Tesseract CLI fallback)      /api/v1/...   API publica
  translate / NER: LLM local (Gemma tres Ollama)       /admin        console operator
  stat: manifest SQLite                              sustenì da: edge SQL DB, object
        │                                              storage (PDFs surant), KV cache
        └── publichea n bundle: SQL + manifest asset + lista de cache-purge ──┘
```

- **Cosc a zero per document te cloud-AI.** OCR y traduzion va localmënter; la maschina a stac ma inant (`scuprì → dejarià → ocr_fat → tradot → publicà`) garantësc che degun document vënie rielaborà, sce no l se à mudà.
- **L corp dla pipeline ne à deguna dependënzes de terzi** — i modui de parsing / manifest / delta va y vën testé sun n Python net zënza nia de instalà cun pip; i stadies de OCR/traduzion se degradescia zënza problems canche i pachec facoltatifs mancia.
- **L sit de bòrd** apliea headers de segurëza stricć + CSP (no `unsafe-inline`; l JSON-LD inline ie fissà cun sha256), negoziazion dla rujeneda tres `Accept-Language` + mapadures per paesc, na cache de plates KV de 30 dis y n cron de mantenimënter al di.
- **Ajunamënc incrementai:** n rilevator de delta afeitea les diferënzes dl'indesc surant y dà ma mudamënc de reviers tla pipeline.

### Per svilupadëures

La API publica sun https://www.ufolens.com/api/v1 dà documënc y metadac coche JSON. L'azès anonim ie limità tla velozità; damandé na clau per i liviei da inrescidëures/svilupadëures. Cialé la sezion API sun l sit per endpoints y limic.

### Stat

Codesc completà; sit metù online sun https://www.ufolens.com. La banca de dac de produzion vën emplenida cun l'esecuzion dla pipeline offline y la publicazion dl bundle inant (`cli_publish run --remote`). I documënc de proiet plens ie te `docs/20260511/`.

### Lizënza / cunfins

- Documënc surant: lëures del guviern federal di Stac Unic, de domini publich ti Stac Unic.
- L codesc de chësta piattaforma: cialé `LICENSE`.
- L sit manda `Tdm-Reservation: 1` y `X-Robots-Tag: noai, noimageai` — da indizé dai muteres de inrescida, cherdà ora dal training/scraping de AI.
- L material video ie atribuì a DVIDS / AARO y ne vën nia revedicà da chësc proiet.

Issues y PRs ie bënunì. Per piazëi liejer `CLAUDE.md` y `docs/20260511/00-*` daniëura da giaurì mudamënc struturai.

