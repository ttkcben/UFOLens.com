# GitHub — Post 1 ri 3 · Paressa / Panyingngereng README

**Mupatunruang:** aga GitHub Release body, aga Panyalai ripakana, ianna punna aga pangkaukeng ri acconna README.
**Kata kuncinna:** UAP, UFO, PURSUE archive, pappada-ada ripasoro' siruntu', data ripasoro', pangkaukeng passiajang gangka' sibawa tuli, OCR, terjemahan mesin, LLM lokal, Ollama, komputasi tepi, API umum, Hono, TypeScript, Python
**Tautan:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — situru' pangkaukeng passiajang gangka' sibawa tuli, mappatunruang data PURSUE UAP archive

**Matana:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Data asali:** https://www.war.gov/ufo

`ufolens.com` mappalettu' poleang **PURSUE** archive UAP / UFO ripasoro' siruntu' ri War Department AS, aga platform pangissengeng: pangkaukeng passiajang gangka' sibawa tuli, terjemahan mesin ri laleng corpus, peta + pangkaukeng tuli ri tawang, sibawa API JSON umum. Pappada-ada asali iyanaritu karya pemerintah federal AS sibawa ri laleng AS iyanaritu domain umum ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Proyekkeng iye **de'na napasilaungang aga pemerintah AS**, de'na napatunruang insignia resmi, sibawa de'na nasibawai ripasoro' sibawa de'na nanyala pangkaukeng.

### Arsitektur

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

- **De'na nanyamei biaya cloud-AI ri tungke' dokument.** OCR sibawa terjemahan makkatenning ri loka; forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) manjamin de'na nanyala dokument riproses poleang risesana punna de'na nanyala agi-agi.
- **Pipeline core de'na nanyala dependensi pihak ketilu** — parsing / manifest / delta modules makkatenning sibawa dites ri Python maccora sibawa de'na nanyala pip-installed; tahap OCR/terjemahan maccora sibawa masagena risesana punna de'na nanyala paket opsional.
- **Edge site** mupatunruang header keamanan ketat + CSP (de'na `unsafe-inline`; inline JSON-LD sha256-pinned), negosiasi basa via `Accept-Language` + peta negara, KV page cache 30 esso, sibawa cron housekeeping tungke' esso.
- **Pembaruan incremental:** delta detector maccora index asali sibawa mappasoro' aga pangkaukeng ri laleng pipeline.

### Untu' developer

API umum ri https://www.ufolens.com/api/v1 mappatunruang dokument sibawa metadata aga JSON. Akses anonim ripassipa' ri patubbu; palettu' kunci untu' tier peneliti/developer. Itai bagian API ri situs untu' endpoint sibawa patubbu.

### Kondisi

Kode gangka'; situs ripasang ri https://www.ufolens.com. Database produksi riisi sibawa makkatenning pipeline offline sibawa mappublish bundle forward (`cli_publish run --remote`). Dokumen desain gangka' engka ri `docs/20260511/`.

### Lisensi / patubbu

- Dokument asali: karya pemerintah federal AS, domain umum ri laleng AS.
- Kode platform iye'e: itai `LICENSE`.
- Situs iye' mappasoro' `Tdm-Reservation: 1` sibawa `X-Robots-Tag: noai, noimageai` — ripa'senggang search engine, ripassiajang poleang AI training/scraping.
- Rekaman video ripatunruang ri DVIDS / AARO sibawa de'na nanyala riklaim ri proyekkeng iye.

Issues sibawa PRs ripassukki. Palettu'i `CLAUDE.md` sibawa `docs/20260511/00-*` ri acconna mappalettu' agi-agi pangkaukeng struktural.
