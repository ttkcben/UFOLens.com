# GitHub — Siaran 3 daripada 3 · Nota seni bina (Perbincangan gaya-ADR)

**Guna sebagai:** Perbincangan di bawah "Tunjuk dan cerita" / "Seni Bina", atau benih ADR `docs/`.
**Kata kunci:** seni bina, ADR, mesin keadaan maju-sahaja, LLM tempatan, Ollama, OCR, pengkomputeran pinggir, CSP, pengepala keselamatan, pipeline data, kejuruteraan kos, manifes SQLite, D1, R2, KV
**Hiperpautan:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mengapa ufolens.com dibina sebegini

Nota mengenai tiga keputusan yang membentuk [ufolens.com](https://www.ufolens.com) (pembinaan semula [arkib PURSUE UAP](https://www.war.gov/ufo) yang boleh dicari dan berbilang bahasa). Komen / tentangan dialu-alukan.

### 1. Pipeline adalah mesin keadaan maju-sahaja — dengan sengaja

Keadaan: `ditemui → dimuat turun → ocr_selesai → diterjemah → diterbitkan`. Sebuah dokumen hanya bergerak ke hadapan, dan hanya apabila ada kerja yang perlu dilakukan. Kandungan yang diterbitkan tidak pernah diproses semula melainkan pengesan delta melihat sumbernya benar-benar berubah.

**Mengapa:** OCR + terjemahan adalah operasi yang mahal, dan arkib berkembang dari semasa ke semasa. Pipeline yang "menjalankan semula semuanya untuk selamat" mempunyai kos yang tidak terhad. Menjadikan peralihan ke belakang mustahil menjadikan bil yang tidak terkawal mustahil. Siling kos adalah sifat graf keadaan, bukan kewaspadaan operator.

**Kos:** penghijrahan skema dan pemprosesan semula dengan sengaja adalah janggal. Pertukaran yang boleh diterima.

### 2. OCR dan terjemahan berjalan pada LLM tempatan, bukan API awan

OCR: enjin sumber terbuka, sandaran Tesseract CLI. Terjemahan + NER: Gemma melalui Ollama, pada komputer riba Apple Silicon.

**Mengapa:** kos marginal sifar bagi setiap dokumen; boleh dihasilkan semula (model + prom tetap); dan langkah pengambilan sudah pun perlu dijalankan dari IP kediaman (sumber berada di belakang Akamai Bot Manager — `curl` mendapat 403), jadi komputer riba tetap berada dalam gelung.

**Kos:** kualiti terjemahan berada di bawah model perbatasan. Untuk korpus rujukan di mana bahasa Inggeris asal sentiasa boleh diakses dengan satu klik, itu tidak mengapa. Kami tidak mendakwa terjemahan itu adalah berwibawa.

### 3. Kedua-dua bahagian berkongsi tepat satu antara muka: berkas yang diterbitkan

Pipeline tidak pernah menulis terus ke pangkalan data pengeluaran. Ia mengeluarkan `{ SQL, manifes aset, senarai pembersihan cache }`. "Menerbitkan" = gunakan berkas itu ke hadapan (tolak SQL ke DB SQL pinggir, segerakkan aset ke storan objek, bersihkan kunci cache yang dinamakan).

**Mengapa:** bahagian tempatan dan bahagian pinggir boleh berkembang secara bebas; berkas itu boleh disemak; dan "gunakan data" mempunyai bentuk yang sama setiap masa. Worker adalah aplikasi TypeScript/Hono yang kecil — CSP yang ketat (tiada `unsafe-inline`; JSON-LD sebaris disemat dengan sha256), perundingan `Accept-Language` + negara→bahasa, cache halaman KV selama 30 hari, cron penyelenggaraan harian — dan ia tidak perlu tahu bagaimana data itu dibuat.

**Kos:** perubahan skema D1 menyentuh dua fail (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurans yang murah.

### Perkara yang tidak boleh dirunding yang terbina dalam tingkah laku

- Tidak bersekutu dengan kerajaan A.S.; tiada lambang rasmi.
- Suntingan sumber dipelihara, tidak pernah diterbalikkan.
- Video diatribusikan kepada DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` di seluruh tapak — boleh diindeks carian, menarik diri daripada pengikisan AI.

Langsung: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

