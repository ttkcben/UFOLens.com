# GitHub — Fasnin ya 1 ye 3 · Ugbãn-yãaka / Lakaolgo la README

**Tũu Woto:** GitHub Ugbãn-yãaka fãmbã, Saglgo panga, walla paasgã ne repo README.
**Wɛɛg-yõdo:** UAP, UFO, PURSUE archive, sɛba a ka teega n-sõngda, open data, sɛb fãa fãa zãms-yãka, OCR, makins-pɛlɛgdgo, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Teeb-yikri:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — a gom-gom pɛlɛg-yãkda, gom-gom tũudma PURSUE UAP sɛb-yõdrã

**Zindgã:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Sɛb-yõdga:** https://www.war.gov/ufo

`ufolens.com` pɛlɛgda War Department (U.S.) **PURSUE** sɛb-yõdrã n ka teeg UAP / UFO sɛb-yõda, n pids gom-gom tũudma: sɛb fãa fãa zãms-yãka, makins-pɛlɛgdgo tẽn-tẽnse fãa, yik-gomdã paasgã ne teeb-yikrã, la public JSON API. Sɛbã yikda War Department (U.S.) gom-gom-minungã n ya public domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Woto ka **gom-gom-minungã ka gom-gom-minungã ne U.S. government**, ka tũu gom-gom-minungã, la ka basga ka sɛbã yikda.

### Architekture

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

- **Ka yĩnde n gũ-da cloud-AI n tũud. OCR la makins-pɛlɛgdgo fãa fãa yikda local;** gom-gom-minungã (`discovered → downloaded → ocr_done → translated → published`) ka tũu n-tũud ka sɛbã ka yikda.
- **Pipeline core ka tũu third-party dependencies** — parsing / manifest / delta modules fãa fãa yikda Python isuma ishakwata ifyacilamo; OCR/translation stages fikalala bwino nga takwaba optional packages.
- **Edge site** tũudda strict security headers + CSP (ka `unsafe-inline`; inline JSON-LD sha256-pinned), gom-gom-minungã (`Accept-Language` + country mapping), 30-day KV page cache, la daily housekeeping cron.
- **Yikri yikri:** delta detector tiligda source index la yikda changesã mu pipeline.

### Ku tũuda-minungã

Public API pa https://www.ufolens.com/api/v1 yikda sɛbã la metadata n ya JSON. Anonymous access ka rate-limited; fwailapo key ku tũuda-minungã / developers. Moneni API section pa site pa endpoints la limits.

### Nyaag-yãaka

Code fãa fãa yikda; site yatwikwa pa https://www.ufolens.com. Production database yakwata ifyacindama mu kubomfya offline pipeline na ukubilisha bundle forward (`cli_publish run --remote`). Full design docs shasangwa mu `docs/20260511/`.

### Gom-gom-minungã / Teeb-yikrã

- Sɛbã yikda War Department (U.S.) gom-gom-minungã, public domain mu U.S.
- Woto gom-gom-minungã code: moneni `LICENSE`.
- Site ituma `Tdm-Reservation: 1` la `X-Robots-Tag: noai, noimageai` — ikasangwako ku search engines, tayakabomfiwe ku AI training/scraping.
- Video footage yatumbikwa ku DVIDS / AARO kabili tayalilwa ku uyu mulimo.

Ifipusho la PRs fyapokelelwa. Palenjeni ukubelenga `CLAUDE.md` na `docs/20260511/00-*` libela ukushilula ifyacinchika fya mu kuilenga.

