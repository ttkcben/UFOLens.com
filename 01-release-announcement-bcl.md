# GitHub — Post 1 of 3 · Anunsyo nin Pagpaluwas / README anunsyo block

**Gamiton bilang:** sarong GitHub Release body, sarong pinned Discussion, o an ibabaw kan repo README.
**Mga Keyword:** UAP, UFO, PURSUE archive, declassified na mga dokumento, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Mga Hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — sarong multilingual, nasesearch na platform para sa PURSUE UAP archive

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Ginikanang archive:** https://www.war.gov/ufo

An `ufolens.com` nagrere-publish kan **PURSUE** archive kan U.S. War Department nin declassified na mga rekord nin UAP / UFO bilang sarong knowledge platform: full-text search, machine translation sa bilog na corpus, map + timeline exploration, asin sarong public JSON API. An ginikanang mga dokumento mga gibo kan pederal na gobyerno kan U.S. asin sa laog kan U.S. pampubliko na domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). An proyektong ini **dae affiliated sa gobyerno kan U.S.**, dae naggagamit nin opisyal na insignia, asin dae nuarinman nagrerebersa nin mga redaksyon.

### Arkitektura

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

- **Zero per-document cloud-AI cost.** An OCR asin translation nagdadalagan lokal; an forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) naggagarantiya na mayo nin dokumento an rerereproseso unless nagliwat ini.
- **An pipeline core mayo nin third-party dependencies** — an mga parsing / manifest / delta module nagdadalagan asin nagtetest sa sarong malinig na Python na mayo nin pip-installed; an mga OCR/translation stage nagdedegrade gracefully pag mayo an opsyonal na mga pakete.
- **An Edge site** naggagamit nin mahigot na mga security header + CSP (mayo nin `unsafe-inline`; an inline JSON-LD sha256-pinned), language negotiation via `Accept-Language` + country mapping, sarong 30-day KV page cache, asin sarong daily housekeeping cron.
- **Incremental na mga update:** sarong delta detector an nagdi-diff sa ginikanang index asin nagpapakan sa mga pagliwat sana pabalik sa pipeline.

### Para sa mga developers

An public API sa https://www.ufolens.com/api/v1 nagrere-return nin mga dokumento asin metadata bilang JSON. An anonymous access rate-limited; magre-request nin key para sa mga researcher/developer tiers. Hilingon an seksyon nin API sa site para sa mga endpoint asin limitasyon.

### Status

Code complete; an site deployed sa https://www.ufolens.com. An production database pinupunan sa paagi nin pagpadalagan kan offline pipeline asin pagpublish kan bundle forward (`cli_publish run --remote`). An kumpletong design docs yaon sa `docs/20260511/`.

### Lisensya / mga limitasyon

- Ginikanang mga dokumento: mga gibo kan pederal na gobyerno kan U.S., public domain sa laog kan U.S.
- An sadiring code kan platform na ini: hilingon an `LICENSE`.
- An site nagpapadara nin `Tdm-Reservation: 1` asin `X-Robots-Tag: noai, noimageai` — indexable kan mga search engine, naka-opt out sa AI training/scraping.
- An video footage attributed sa DVIDS / AARO asin dae kinla-claim kan proyektong ini.

An mga isyu asin PRs welcome. Tabing basahon an `CLAUDE.md` asin `docs/20260511/00-*` bago magbukas nin structural changes.

