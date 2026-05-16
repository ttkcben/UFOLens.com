# GitHub — Post 1 of 3 · Jeheka / README jeheka oñembojokóva

**Oñeñehaʼãva:** peteĩ GitHub Release, peteĩ Ñe’ẽmbyry oñembojokóva, térã pe repo README yvate gotyo.
**Ñe’ẽrakuã:** UAP, UFO, PURSUE rembiapokue, kuatiañe’ẽ oñemboguejýva, marandu ojepe’áva, jeheka henyhẽva, OCR, ñe’ẽkatu pytyvõ, LLM tenda, Ollama, apopyrã yképe, API rehegua, Hono, TypeScript, Python
**Ñembojoaju:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — peteĩ ñe'ẽeta, jeheka ikatúva ojejapo PURSUE UAP rembiapokuére

**Hetaiteve:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Oñemohu'ãva:** https://www.war.gov/ufo

`ufolens.com` ombopublica jey U.S. War Department **PURSUE** rembiapokue UAP / UFO kuatiañe’ẽ oñemboguejýva peteĩ marandu aty ramo: jeheka henyhẽva, ñe’ẽkatu pytyvõ oparupi, mapa + ára rapo jehecha, ha peteĩ JSON API maymáva oipurukuaáva. Umi kuatiañe’ẽ oúva pe U.S. federal gobierno rembiapópegui ha U.S. ryepýpe katuete ojeheka ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ko tembiapo **ndojokuái U.S. gobierno-pe**, ndoipurúi ñembo’e poravopyre, ha araka’eve ndojereói umi ñembo’e oñembotapykúva.

### Ñemopu’ã

```
Local machine (Apple Silicon, residential IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Zero per-document cloud-AI costo.** OCR ha ñe’ẽkatu pytyvõ oñeha’ã tendápe; pe forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) ombohepy ndojepurui jey hag̃ua mba’eve kuatiañe’ẽ oñembopyaháva’ỹramo.
- **Pipeline core ndorekói mbohapyha dependencia** — parsing / manifest / delta módulos oñeha’ã ha oñeha’ã Python opu’akáva reheve pip-install ndaipóriva; OCR/ñe’ẽkatu pytyvõ oñemboguejýva porã oñemopy’ỹi umi paquetes opcional oĩ’ỹramo.
- **Edge sitio** omoĩ peteĩ security headers + CSP mbarete (no `unsafe-inline`; inline JSON-LD sha256-pinned), ñe’ẽkatu negociacion `Accept-Language` + tetã ñembojoaju rupi, peteĩ 30 ara KV page cache, ha peteĩ housekeeping cron ára jave.
- **Ñembopyahu michĩva:** peteĩ delta detector ohecha pe source index ha ombohasa jey umi ñembopyahu añónte pe pipeline-pe.

### Devlopérape g̃uarã

Pe API maymáva oipurukuaáva https://www.ufolens.com/api/v1 ombojevy kuatiañe’ẽ ha metadata JSON-pe. Acceso anónimo oreko límite; ejerure peteĩ clave researcher/developer tier-pe g̃uarã. Ehecha API sección pe sitio-pe umi endpoint ha límites-pe g̃uarã.

### Estado

Código oñemohu’ã; sitio oñemondo https://www.ufolens.com-pe. Pe producción database oñembojy pe offline pipeline oñeha’ãvo ha pe bundle oñembopublica tenonde gotyo (`cli_publish run --remote`). Documentos de diseño henyhẽva oĩ `docs/20260511/`-pe.

### Licencia / límites

- Fuente kuatiañe’ẽ: U.S. federal gobierno rembiapokue, dominio público U.S. ryepýpe.
- Ko plataforma código: ehecha `LICENSE`.
- Pe sitio omondo `Tdm-Reservation: 1` ha `X-Robots-Tag: noai, noimageai` — jeheka kuatiakuéra ohechakuaáva, ojeipe’a AI entrenamiento/scraping-gui.
- Video oñembohasa DVIDS / AARO-pe ha ndaha’éi ko proyecto mba’e.

Issues ha PRs ojehechakuaa. Emoñe’ẽ `CLAUDE.md` ha `docs/20260511/00-*` reipe’a mboyve ñembopyahu estructura rehegua.
