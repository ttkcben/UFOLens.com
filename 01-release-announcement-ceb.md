# GitHub — Post 1 of 3 · Pagpagawas / README nga pahibalo nga block

**Gamiton isip:** usa ka GitHub Release body, usa ka pinned Discussion, o ang ibabaw sa repo README.
**Mga Keywords:** UAP, UFO, PURSUE archive, gipagawas nga mga dokumento, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Mga Hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — usa ka multilingual, mahimong pangitaon nga plataporma alang sa PURSUE UAP archive

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Source archive:** https://www.war.gov/ufo

Ang `ufolens.com` nag-republish sa U.S. War Department's **PURSUE** archive sa declassified UAP / UFO records isip usa ka knowledge platform: full-text search, machine translation sa tibuok corpus, pag-explore sa mapa + timeline, ug usa ka public JSON API. Ang mga source document kay mga buhat sa U.S. federal government ug sulod sa U.S. kay public domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Kini nga proyekto **dili konektado sa U.S. government**, walay gigamit nga opisyal nga insignia, ug dili gayud magbalit-ad sa mga redaction.

### Arkitektura

```
Local machine (Apple Silicon, residential IP) Edge network
───────────────────────────────────────── ─────────────────────────
pipeline/ (Python 3.10, stdlib-only core) worker/ (TypeScript, Hono.js)
  fetch → OCR → translate → publish (forward-only) /{lang}/... pages
  OCR: open-source engine (Tesseract CLI fallback) /api/v1/... public API
  translate / NER: local LLM (Gemma via Ollama) /admin operator console
  state: SQLite manifest backed by: edge SQL DB, object
        │ storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Walay gasto sa cloud-AI matag dokumento.** Ang OCR ug translation nagdagan sa lokal; ang forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) nagagarantiya nga walay dokumento nga ma-reprocess gawas kung kini nausab.
- **Ang pipeline core walay third-party dependencies** — ang parsing / manifest / delta modules nagdagan ug nag-test sa usa ka limpyo nga Python nga walay pip-installed; ang OCR/translation stages grasyoso nga madaot kung ang optional packages wala.
- **Ang edge site** nag-apply og estrikto nga security headers + CSP (walay `unsafe-inline`; ang inline JSON-LD sha256-pinned), language negotiation pinaagi sa `Accept-Language` + country mapping, usa ka 30-day KV page cache, ug usa ka daily housekeeping cron.
- **Incremental updates:** usa ka delta detector ang nag-diff sa source index ug nagpakaon lang sa mga kausaban balik sa pipeline.

### Para sa mga developers

Ang public API sa https://www.ufolens.com/api/v1 nagbalik og mga dokumento ug metadata isip JSON. Ang anonymous access kay rate-limited; pagpangayo og key para sa researcher/developer tiers. Tan-awa ang API section sa site para sa mga endpoint ug mga limitasyon.

### Status

Kompleto na ang code; ang site gi-deploy na sa https://www.ufolens.com. Ang production database gipuno pinaagi sa pagpadagan sa offline pipeline ug pag-publish sa bundle forward (`cli_publish run --remote`). Ang kompleto nga design docs anaa sa `docs/20260511/`.

### Lisensya / mga utlanan

- Source documents: Mga buhat sa U.S. federal government, public domain sulod sa U.S.
- Ang kaugalingong code niini nga plataporma: tan-awa ang `LICENSE`.
- Ang site nagpadala og `Tdm-Reservation: 1` ug `X-Robots-Tag: noai, noimageai` — mahimong ma-index sa mga search engine, gipili nga dili alang sa AI training/scraping.
- Ang footage sa video gi-attribute sa DVIDS / AARO ug dili gi-claim niini nga proyekto.

Ang mga isyu ug PRs gidawat. Palihug basaha ang `CLAUDE.md` ug `docs/20260511/00-*` sa dili pa mag-abli sa mga structural changes.

