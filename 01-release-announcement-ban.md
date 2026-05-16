# GitHub — Post 1 of 3 · Release / README announcement block

**Anggen dados:** Pamargin rilis GitHub, Siki diskusi pinaka semat, utawi ring pucuk README genah repone.
**Kruna-kruna kunci:** UAP, UFO, arsip PURSUE, dokumen sané sampun karesmian, data tebuka, panyelehan teks jangkep, OCR, terjemahan mesin, LLM lokal, Ollama, komputasi pinggir, API umum, Hono, TypeScript, Python
**Praktek pranala:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — siki platform makarya sareng makudang basa, sané prasida kasayabin antuk arsip PURSUE UAP

**Mangda ngaksi:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Arsip sumber:** https://www.war.gov/ufo

`ufolens.com` nyobiahang malih arsip U.S. War Department sané kasumbung antuk **PURSUE** rerincian UAP / UFO sané sampun karesmian dados platform kaweruhan: panyelehan teks jangkep, terjemahan mesin ring sajeroning korpus, peta + panyelehan gala, miwah siki API JSON umum. Dokumen sumber puniki kakaryanin olih pamréntah federal AS tur ring AS inggih punika widang umum ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Proyek puniki **nenthang pisan kakait sareng pamréntah AS**, nenten nganggen lambang resmi, tur nenten pisan ngembalikan penyensoran.

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

- **Nenten wenten biaya cloud-AI yéning ngitung per-dokumen.** OCR miwah terjemahan kalaksanayang secara lokal; mesin keadaan sané maju-terus (`discovered → downloaded → ocr_done → translated → published`) njamin nenten wenten dokumen sané kawiwitin malih yéning nenten wenten perubahan.
- **Inti pipeline nenten maderbé dependensi pihak katiga** — modul parsing / manifest / delta kalaksanayang miwah kauji ring Python sané resik tanpa wenten pip sané kainstal; tahapan OCR/terjemahan prasida kakirangin kualitasnyané yéning paket opsional nenten wenten.
- **Situs edge** nerapkan header keamanan sané ketat + CSP (nenten `unsafe-inline`; inline JSON-LD sha256-pinned), negosiasi basa malantaran `Accept-Language` + pemetaan negara, siki cache halaman KV 30 dina, miwah siki cron housekeeping nyabran rahina.
- **Pembaruan bertahap:** siki detektor delta ngawilah indeks sumber miwah ngasupang wantah perubahan sané kauningin ring pipeline.

### Majeng para pangembang

API umum ring https://www.ufolens.com/api/v1 ngembalikan dokumen miwah metadata dados JSON. Akses anonim kalaksanayang antuk wates kacepatan; ngaptiang kunci majeng tingkatan panaliti/pangembang. Wacen segmen API ring situs puniki majeng titik akhir miwah wates.

### Status

Kode sampun jangkep; situs sampun kaunggah ring https://www.ufolens.com. Basis data produksi kaisi antuk ngalaksanayang pipeline offline miwah nyobiahang paket (`cli_publish run --remote`). Dokumen desain jangkep wenten ring `docs/20260511/`.

### Lisensi / Watesan

- Dokumen sumber: pakaryan pamréntah federal AS, widang umum ring AS.
- Kode platform puniki sorang: wacen `LICENSE`.
- Situs puniki ngirim `Tdm-Reservation: 1` miwah `X-Robots-Tag: noai, noimageai` — prasida kaidéntifikasi olih mesin panyelehan, nenten kasarengin ring pelatihan/panyelehan AI.
- Rekaman video kakait sareng DVIDS / AARO miwah nenten kakuasayang olih proyek puniki.

Isu miwah PR kaaptiang. Sangkaning punika, wacen `CLAUDE.md` miwah `docs/20260511/00-*` sadurung ngawitin perubahan struktur.

