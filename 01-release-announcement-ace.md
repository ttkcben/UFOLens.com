# GitHub — Buleuen Keu-1 nibak 3 · Peuneugah rilis / peuneugah README block

**Keugunaan:** Badan Rilis GitHub, Diskusi nyang geupinyèk, atawa bagian ateuh nibak README repo.
**Kunci Kata:** UAP, UFO, arsip PURSUE, deklasifikasi dokumèn, data teubuka, peugléh-teukléh nyang peunoh, OCR, peuneugah mesin, LLM lokal, Ollama, komputasi tepi, API umum, Hono, TypeScript, Python
**Hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — saboh platform meubasa, meuteumé crôm keu arsip PURSUE UAP

**Udep:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Arsip sumber:** https://www.war.gov/ufo

`ufolens.com` meupublikasi ulang arsip **PURSUE** deklasifikasi reukod UAP / UFO nibak Departemen Peurang AS seubagoe platform peungetahuan: peugléh-teukléh nyang peunoh, peuneugah mesin bak mandum korpus, peta + éksplorasi garih masa, ngön API JSON umum. Dokumèn sumber nyan buet nibak pemerintah federal AS ngön lam AS nyan milék umum ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Proyek nyoë **hana meuikat ngön pemerintah AS**, hana geungui tanda resmi, ngön hana geupeubateu ulang nyang ka geupeungön.

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

- **Hana biaya cloud-AI keu tiëp dokumèn.** OCR ngön peuneugah geujak bak lokal; mesin neugara maju-mantong (`geuteumé → geunduh → ocr_done → geupeuneugah → geupeuteubiet`) geujamin hana dokumèn nyang geuproses ulang meunyoe hana meu'ubah.
- **Inti pipeline hana deupèndènsi pihak keulhèe** — modul analisis / manifès / delta geujak ngön geujiyiëk bak Python nyang peugléh ngön hana peu-peu nyang geu-instal nibak pip; tahap OCR/peuneugah geuturon meunyoe paket opsional hana na.
- **Situs tepi** geupeuguna header keamanan nyang keunét + CSP (hana `unsafe-inline`; inline JSON-LD sha256-geupinyèk), negosiasi basa lé `Accept-Language` + peta neugara, cache halaman KV 30 uroë, ngön cron beurèh uroë-uroë.
- **Pembaruan incremental:** detektor delta geupeubida indeks sumber ngön geupeusoe mantong ubahan keu pipeline.

### Keu pengembang

API umum bak https://www.ufolens.com/api/v1 geupeuriyeuk dokumèn ngön metadata sabagoe JSON. Akses anonim na batasan laju; neupeulakèe kunci keu tingkeue peneliti/pengembang. Neupeuliyët bagian API bak situs keu éndpoint ngön batasan.

### Status

Kode ka leungkap; situs ka geupeugléh bak https://www.ufolens.com. Basis data produksi geupeusoe lé geujak pipeline offline ngön geupeuteubiet bundel maju (`cli_publish run --remote`). Dokumèn desain peunoh na lam `docs/20260511/`.

### Lisensi / Batasan

- Dokumèn sumber: buet pemerintah federal AS, milék umum lam AS.
- Kode platform nyoë keudroë: neupeuliyët `LICENSE`.
- Situs geukirém `Tdm-Reservation: 1` ngön `X-Robots-Tag: noai, noimageai` — jeuët geuindeks lé mesin peugléh, geupeulheuëh nibak pelatihan/ékstraksi AI.
- Rekaman video geu-atribusikan keu DVIDS / AARO ngön hana geuklaim lé proyek nyoë.

Masalah ngön PR geupeurayok. Neupeubaca `CLAUDE.md` ngön `docs/20260511/00-*` seugolom neupeuhah ubahan struktural.

