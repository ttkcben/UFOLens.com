# GitHub — Kimsa · Katuqata / README yatiyañ qillqa

**Uñacht'ayañataki:** GitHub-an katuqatawa, may jach'a amuyuwa, jan ukax repositorio README ukan qalltawiwa.
**Jach'a aru:** UAP, UFO, PURSUE archivo, ch'uqt'ata qillqatanaka, jach'a yatiwi, maypach qillqa thaqhaña, OCR, makina jaqichaña, local LLM, Ollama, edge computing, pública API, Hono, TypeScript, Python
**Chiqawaya:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — may jaqichata, thaqhañjam platforma PURSUE UAP archivonakataki

**Jach'a:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Qallta archivo:** https://www.war.gov/ufo

`ufolens.com` mayamp ch'uqt'ayi U.S. War Department ukan **PURSUE** UAP / UFO records archivos ch'uqt'atpacha, may yatiwi platformar tukuyañataki: maypach qillqa thaqhaña, maypach archivo ukan makina jaqichawi, mapa + pacha thaqhaña, ukhamarak maypach JSON API. Qallta qillqatanaka U.S. federal gobierno ukan lurañanakapawa ukat U.S. markana jach'a uñakipt'añataki (17 U.S.C. §105). Akax projectowa **U.S. gobierno ukampi jan mayacht'atawa**, janiw oficial insignia apnaqiti, ukat janiw ch'uqt'ata qillqatanak jisk'aptayiñxati.

### Architecture

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

- **Janiw maypach qillqataki nube-AI qullqi utjkiti.** OCR ukat jaqichawi local-n irnaqi; jach'a thakhinak sarañaki (`discovered → downloaded → ocr_done → translated → published`) janiw maypach qillqa mayamp irnaqatäñapäkiti jan ukax mayjt'ayataxati.
- **Pipeline core janiw maypach dependencia utjkiti** — parsing / manifest / delta módulos irnaqi ukat mayqhaw python ukan testiya, janiw pip-an install-ataxati; OCR/translation etapas janiw maypach paquetes utjkipansti jisk'aptayiñxati.
- **Edge sitio** jach'a seguridad header-kunampi + CSP-mpi (`unsafe-inline` janiw; inline JSON-LD sha256-n pin-ata), `Accept-Language` + markana jaqichawi apnaqasa, 30 uru KV page cache, ukat maypach uru wakichawi cron.
- **Jach'aptayañanakata:** may delta detector ukanak uñakipt'ayi ukat maypach mayjt'ayatanakakiw pipeline-r phokhasi.

### Desarrolladoranakataki

https://www.ufolens.com/api/v1 ukan jach'a API qillqatanaka ukat metadata JSON ukham kutt'ayi. Anonimo acceso ukan limitatatawa; investigador / desarrollador nivelanakataki may llave mayiñama. API sección ukan sitio ukan jach'a thakhinak ukat limitanak uñakipt'añama.

### Estado

Code ukax tukuyañatawa; sitio ukax https://www.ufolens.com ukanw uñst'ata. Producción base de datos ukax offline pipeline-n irnaqayatawa ukat bundle ukax jach'ar uñt'ayatawa (`cli_publish run --remote`). Jach'a diseño docs ukax `docs/20260511/` ukanw utji.

### Licencia / juchanakapa

- Qallta qillqatanaka: U.S. federal gobierno ukan lurañanakapawa, U.S. markan jach'a uñakipt'añataki.
- Akax plataforma ukan código ukaxa: `LICENSE` uñakipt'añama.
- Sitio ukax `Tdm-Reservation: 1` ukat `X-Robots-Tag: noai, noimageai` apayi — thaqhañ motoranakax uñakipt'añapatakiwa, AI luraña / qillqaña janiw irnaqayiti.
- Video ukax DVIDS / AARO ukanakwa; akax projecto janiw jupan lurañapäkiti.

Juchanak ukat PRs ukax sumawa. `CLAUDE.md` ukat `docs/20260511/00-*` uñakipt'añamawa maypach mayjt'ayañataki PR janïr qalltañkamax.

