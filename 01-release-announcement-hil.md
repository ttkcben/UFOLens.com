# GitHub — Post 1 sang 3 · Pagpagwa / README nga pahibalo nga bloke

**Gamiton bilang:** isang GitHub Release body, isang pinned Discussion, ukon ang ibabaw sang repo README.
**Keywords:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — isa ka multilingual, mahimo-pangitaon nga plataporma para sa PURSUE UAP archive

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Ginhalinan nga archive:** https://www.war.gov/ufo

Ang `ufolens.com` nagpagwa liwat sang **PURSUE** archive sang U.S. War Department sang declassified UAP / UFO records bilang isa ka knowledge platform: full-text search, machine translation sa bilog nga corpus, mapa + timeline exploration, kag isa ka public JSON API. Ang ginhalinan nga mga dokumento mga obra sang pederal nga gobyerno sang U.S. kag sa sulod sang U.S. mga public domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ini nga proyekto **wala nagakonektar sa gobyerno sang U.S.**, wala nagagamit sang opisyal nga insignias, kag wala nagabaliskad sang mga redactions.

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

- **Zero per-document cloud-AI cost.** Ang OCR kag translation nagadalagan lokal; ang forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) nagagarantiya nga wala sang dokumento nga iproseso liwat luwas kon ini nagbag-o.
- **Ang pipeline core wala sang third-party dependencies** — ang parsing / manifest / delta modules nagadalagan kag ginatesting sa isa ka matinlo nga Python nga wala sang pip-installed; ang OCR/translation stages nagadaogdaug sing mahinay kon wala ang opsyonal nga packages.
- **Ang Edge site** nagagamit sang estrikto nga security headers + CSP (wala sang `unsafe-inline`; ang inline JSON-LD sha256-pinned), language negotiation via `Accept-Language` + country mapping, isa ka 30-day KV page cache, kag isa ka daily housekeeping cron.
- **Incremental updates:** ang isa ka delta detector nagadiffer sa source index kag nagahatag lang sang mga pagbag-o pabalik sa pipeline.

### Para sa mga developers

Ang public API sa https://www.ufolens.com/api/v1 nagabalik sang mga dokumento kag metadata bilang JSON. Ang anonymous access may rate-limited; magpangayo sang key para sa researcher/developer tiers. Tan-awa ang API section sa site para sa mga endpoints kag limits.

### Status

Ang code kumpleto na; ang site deployed sa https://www.ufolens.com. Ang production database ginapopulate paagi sa pagpadalagan sang offline pipeline kag pagpublish sang bundle forward (`cli_publish run --remote`). Ang kumpleto nga design docs ara sa `docs/20260511/`.

### License / mga limitasyon

- Ginhalinan nga mga dokumento: mga obra sang pederal nga gobyerno sang U.S., public domain sa sulod sang U.S.
- Ang kaugalingon nga code sini nga plataporma: tan-awa ang `LICENSE`.
- Ang site nagapadala sang `Tdm-Reservation: 1` kag `X-Robots-Tag: noai, noimageai` — mahimo i-index sang search engines, wala ginaugyunan nga i-AI training/scraping.
- Ang video footage gin-attribute sa DVIDS / AARO kag wala ginaangkon sang sini nga proyekto.

Ang mga isyu kag PRs ginabaton. Palihug basaha ang `CLAUDE.md` kag `docs/20260511/00-*` antes magbukas sang structural changes.

