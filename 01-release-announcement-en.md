# GitHub — Post 1 of 3 · Release / README announcement block

**Use as:** a GitHub Release body, a pinned Discussion, or the top of the repo README.
**Keywords:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — a multilingual, searchable platform for the PURSUE UAP archive

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Source archive:** https://www.war.gov/ufo

`ufolens.com` re-publishes the U.S. War Department's **PURSUE** archive of declassified UAP / UFO records as a knowledge platform: full-text search, machine translation across the corpus, map + timeline exploration, and a public JSON API. Source documents are works of the U.S. federal government and within the U.S. are public domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). This project is **not affiliated with the U.S. government**, uses no official insignia, and never reverses redactions.

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

- **Zero per-document cloud-AI cost.** OCR and translation run locally; the forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) guarantees no document is reprocessed unless it changed.
- **Pipeline core has no third-party dependencies** — parsing / manifest / delta modules run and test on a clean Python with nothing pip-installed; OCR/translation stages degrade gracefully when optional packages are absent.
- **Edge site** applies strict security headers + CSP (no `unsafe-inline`; inline JSON-LD sha256-pinned), language negotiation via `Accept-Language` + country mapping, a 30-day KV page cache, and a daily housekeeping cron.
- **Incremental updates:** a delta detector diffs the source index and feeds only changes back into the pipeline.

### For developers

The public API at https://www.ufolens.com/api/v1 returns documents and metadata as JSON. Anonymous access is rate-limited; request a key for researcher/developer tiers. See the API section on the site for endpoints and limits.

### Status

Code complete; site deployed at https://www.ufolens.com. The production database is populated by running the offline pipeline and publishing the bundle forward (`cli_publish run --remote`). Full design docs live in `docs/20260511/`.

### License / boundaries

- Source documents: U.S. federal government works, public domain within the U.S.
- This platform's own code: see `LICENSE`.
- The site sends `Tdm-Reservation: 1` and `X-Robots-Tag: noai, noimageai` — indexable by search engines, opted out of AI training/scraping.
- Video footage is attributed to DVIDS / AARO and is not claimed by this project.

Issues and PRs welcome. Please read `CLAUDE.md` and `docs/20260511/00-*` before opening structural changes.
