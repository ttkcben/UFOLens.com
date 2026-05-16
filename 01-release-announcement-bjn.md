# GitHub — Pos 1 matan 3 · Pengumuman Rilis / README

**Gunaakan gasan:** badan Rilis GitHub, Diskusi nang dipin, atawa bagian atas README repo.
**Kata Kunci:** UAP, UFO, arsip PURSUE, dukumin dideklasifikasi, data tabuka, pancarian teks langkap, OCR, tarjamahan masin, LLM lokal, Ollama, kumputasi edge, API publik, Hono, TypeScript, Python
**Hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — platpum babasa bamacam-macam wan kawa dicari gasan arsip PURSUE UAP

**Siap Guna:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Arsip Sumber:** https://www.war.gov/ufo

`ufolens.com` manarbitakan pulang arsip **PURSUE** Departemen Perang AS nang baasal matan rikaman UAP / UFO nang dideklasifikasi sabagai platpum pangatahuan: pancarian teks langkap, tarjamahan masin di saluruh korpus, eksplorasi peta + garis waktu, wan API JSON publik. Dukumin sumbar marupakan karya pamarintah federal AS wan di dalam AS marupakan ranah publik ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Parujekan ngini **kada bapisit lawan pamarintah AS**, kada mamakai insignia rasmi, wan kada suah mambulikakan redaksi.

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

- **Biaya cloud-AI zero par dukumin.** OCR wan tarjamahan bajalan sacara lokal; masin kaadaan maju haja (`discovered → downloaded → ocr_done → translated → published`) manjamin kadada dukumin nang diproses pulang kacuali baubah.
- **Inti pipeline kada baisi depedensi pihak katiga** — modul parsing / manifest / delta bajalan wan diuji di Python nang barasih lawan kadada pip nang tainstal; tahapan OCR/tarjamahan bakurang kualitasnya sacara anggun amun paket opsional kadada.
- **Situs Edge** mamanfaatakan header kaamanan nang katat + CSP (kadada `unsafe-inline`; JSON-LD inline sha256-dipin), negosiasi basa malalui `Accept-Language` + pamanjangan nagara, cache halaman KV 30 hari, wan cron rumah tangga harian.
- **Pambaharuan inkremental:** detektor delta mambandingakan indék sumbar wan mamasukakan parubahan haja pulang ka pipeline.

### Gasan panggawi

API publik di https://www.ufolens.com/api/v1 mambulikakan dukumin wan metadata sabagai JSON. Akses anonim dibatasi lajunya; minta kunci gasan tingkatan panaliti/panggawi. Itih bagian API di situs gasan endpoint wan batasan.

### Status

Kode langkap; situs sudah tadiri di https://www.ufolens.com. Database pruduksi diisi lawan manjalanakan pipeline luring wan manarbitakan bunderan maju (`cli_publish run --remote`). Dukumin disain langkap ada di `docs/20260511/`.

### Lisensi / Batasan

- Dukumin sumbar: karya pamarintah federal AS, ranah publik di dalam AS.
- Kode platpum ngini saurang: itih `LICENSE`.
- Situs maantar `Tdm-Reservation: 1` wan `X-Robots-Tag: noai, noimageai` — kawa diindék ulih masin pancari, ditolak matan palatihan/scraping AI.
- Video footage dikreditakan ka DVIDS / AARO wan kada diklaim ulih parujekan ngini.

Masalah wan PR disambat. Harap baca `CLAUDE.md` wan `docs/20260511/00-*` sabalum mambuka parubahan struktural.
