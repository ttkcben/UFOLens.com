# GitHub — Post 1 n 3 · Blok n anons n Release / README

**Amezdeg a:** GitHub Release body, pinned Discussion, negh t-top n repo README.
**Iwalen-imaziwen:** UAP, UFO, PURSUE archive, imuran i-desclasifiyen, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — platform multilingüe d t-search n PURSUE UAP archive

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Source archive:** https://www.war.gov/ufo

`ufolens.com` it-re-publishes archive n **PURSUE** n U.S. War Department n imuran i-desclasifiyen UAP / UFO am knowledge platform: full-text search, machine translation across corpus, map + timeline exploration, d public JSON API. Imuran n source d works n U.S. federal government d public domain n U.S. ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Project-agi **ur-it-affili-ed s U.S. government**, ur-it-usa official insignia, d ur-it-reverses redactions.

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

- **Zero per-document cloud-AI cost.** OCR d translation it-run locally; forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) it-guarantees belli ur-it-reprocess-e document unless it-changed.
- **Pipeline core ur-it-has third-party dependencies** — parsing / manifest / delta modules it-run d it-test-e s clean Python without pip-installed; OCR/translation stages it-degrade gracefully m-optional packages ur-it-present.
- **Edge site** it-applies strict security headers + CSP (no `unsafe-inline`; inline JSON-LD sha256-pinned), language negotiation via `Accept-Language` + country mapping, 30-day KV page cache, d daily housekeeping cron.
- **Incremental updates:** delta detector it-diffs source index d it-feeds only changes back s pipeline.

### For developers

Public API n https://www.ufolens.com/api/v1 it-returns documents d metadata am JSON. Anonymous access it-rate-limited; request-e key for researcher/developer tiers. Zure-e API section n site for endpoints d limits.

### Status

Code complete; site deployed n https://www.ufolens.com. Production database it-populated s running offline pipeline d publishing bundle forward (`cli_publish run --remote`). Full design docs it-live n `docs/20260511/`.

### License / boundaries

- Source documents: U.S. federal government works, public domain n U.S.
- Code n platform-agi: zure-e `LICENSE`.
- Site it-sends `Tdm-Reservation: 1` d `X-Robots-Tag: noai, noimageai` — indexable s search engines, opted out n AI training/scraping.
- Video footage it-attributed s DVIDS / AARO d ur-it-claimed s project-agi.

Issues d PRs welcome. Zure-e `CLAUDE.md` d `docs/20260511/00-*` before opening structural changes.