# GitHub — Post 1 ta 3 · Sakiya / README famiya block

**A na taa yi kamar:** GitHub Release body, a pinned Discussion, ko repo README so.
**Keywords:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — wande ka taa ma'ana, PURSUE UAP archive goono ka taa gude platform

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Source archive:** https://www.war.gov/ufo

`ufolens.com` U.S. War Department ta **PURSUE** archive ka UAP / UFO taa records ka sakiya, knowledge platform ye: full-text search, machine translation corpus goono, map + timeline exploration, o public JSON API. Source documents U.S. federal government taa ce, U.S. goono public domain ye ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Projekt wande **U.S. government da ba ya** no, insignia ba na taa, o redactions ba ya jireya.

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

- **Zero per-document cloud-AI cost.** OCR o translation local goono ka taa; forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) ba ya guarantee ka document ba na taa reprocess ba, sai ka a ya canza.
- **Pipeline core has no third-party dependencies** — parsing / manifest / delta modules clean Python goono ka taa test, pip-installed ba na taa; OCR/translation stages gracefully degrade ka optional packages ba na taa.
- **Edge site** strict security headers + CSP ba na taa (`no unsafe-inline`; inline JSON-LD sha256-pinned), language negotiation `Accept-Language` + country mapping goono, 30-day KV page cache, o daily housekeeping cron.
- **Incremental updates:** delta detector source index goono ka taa diff, o changes goono ka taa feed pipeline goono.

### Developers ga

Public API wande https://www.ufolens.com/api/v1 documents o metadata JSON ye ka taa sakiya. Anonymous access rate-limited no; key ye request researcher/developer tiers ga. API section goono site goono ka taa taa endpoints o limits ga.

### Status

Code complete no; site https://www.ufolens.com goono ka taa deploy. Production database offline pipeline goono ka taa run, o bundle forward goono ka taa publish (`cli_publish run --remote`). Full design docs `docs/20260511/` goono.

### License / boundaries

- Source documents: U.S. federal government taa ce, public domain U.S. goono.
- Platform wande taa code: `LICENSE` goono ka taa taa.
- Site `Tdm-Reservation: 1` o `X-Robots-Tag: noai, noimageai` ba na taa sakiya — search engines goono ka taa index, AI training/scraping goono ka taa opt out.
- Video footage DVIDS / AARO goono ka taa attribute, o projekt wande ba na taa claim.

Issues o PRs welcome no. `CLAUDE.md` o `docs/20260511/00-*` goono ka taa taa structural changes goono ka taa open goono goono.

