# GitHub — Dixi 1 dia 3 · Kusazola / Mukanda wa kixibulu wa README

**Sadisa kala:** Mukuta wa GitHub Release, Makani masokeka, mba kulu dia README dia repo.
**Tanga yakwiza:** UAP, UFO, PURSUE archive, mikanda miabula ku dibulu dia moko, data yabula, kusaka kwa tanga ya yoso, OCR, kumanyisa kwa ngalu, LLM ya dibulu, Ollama, edge computing, API yabula, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — disu dingi dia malaka, disaka, dia laka ya zungulu ku dibulu dia PURSUE UAP

**Kiakamwene:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Dibulu dia kifunkula:** https://www.war.gov/ufo

`ufolens.com` yazola diaka dibulu dia **PURSUE** dia Departementu ya Kialwa ya EUA dia mikanda miabula ku dibulu dia UAP / UFO kala disu dia zayi: kusaka kwa tanga ya yoso, kumanyisa kwa ngalu mu disu diayoso, mapu + kuyenda ku makani ma nsungi, na API ya JSON yabula. Mikanda ya kifunkula misala mia kumbi dia guvernu dia EUA na mu EUA mina mu dibulu dia moko ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Nkalu iyi **kana ya nwanana na guvernu dia EUA**, kaysadisi masumu ma ngalwa, na kana yafungula ka redakisa.

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

- **Zero nzimbu ya cloud-AI pa mukanda umosi.** OCR na kumanyisa miasala ku dibulu; ngalu ya state machine ya kuyenda ku ntwala (`discovered → downloaded → ocr_done → translated → published`) yasikidisanga kanda mukanda umosi usangulwasa diaka kana wacanjiki.
- **Pipeline core kaykweti yakwiza ya third-party** — parsing / manifest / delta modules miasala na mipimbwa ku Python ya nswakani kaykweti pip-installed; OCR/translation stages mikolama bwino kana packages za nsokela kana zibwidi.
- **Edge site** yasikadisanga security headers ya ngolo + CSP (kana `unsafe-inline`; inline JSON-LD sha256-pinned), laka ya kubwika kubwika Accept-Language + kuyala ku nsi, KV page cache ya malaka 30, na cron ya kusadisa ku naku.
- **Incremental updates:** delta detector yakwanisanga index ya kifunkula na yasakidi fwayidi yakwiza fwayidi ya pipeline.

### Ku bantu bakudisa

API yabula ku https://www.ufolens.com/api/v1 yakweti mikanda na metadata kala JSON. Kuzola kwa anonimu kuli na rate-limited; sakila key ku bantu bakweti kusaka / bantu bakweti kusadisa. Tala API section ku site ku endpoints na limits.

### Nswa

Code yamana; site yatelamene ku https://www.ufolens.com. Database ya kupangidisa yaminina ku kusala pipeline kaykweti kusadisa na kusazola bundle ku ntwala (`cli_publish run --remote`). Mikanda ya design ya yoso mivulwa mu `docs/20260511/`.

### License / mifumbulu

- Mikanda ya kifunkula: misala mia kumbi dia guvernu dia EUA, dibulu dia moko mu EUA.
- Code ya disu diyi: tala `LICENSE`.
- Site yatuma `Tdm-Reservation: 1` na `X-Robots-Tag: noai, noimageai` — isaka ku search engines, kaykweti AI training/scraping.
- Video footage yatakama ku DVIDS / AARO na kana yakwata ku nkalu iyi.

Issues na PRs mibwidi. Bala `CLAUDE.md` na `docs/20260511/00-*` libwidi libwidi kuvula mambulu ma ngalwa.

