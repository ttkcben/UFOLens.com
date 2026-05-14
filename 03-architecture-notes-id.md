# GitHub — Kiriman 3 dari 3 · Catatan arsitektur (Diskusi gaya-ADR)

**Gunakan sebagai:** Diskusi di bawah "Pamer dan cerita" / "Arsitektur", atau bibit ADR `docs/`.
**Kata kunci:** arsitektur, ADR, mesin keadaan maju-saja, LLM lokal, Ollama, OCR, komputasi tepi, CSP, header keamanan, pipeline data, rekayasa biaya, manifes SQLite, D1, R2, KV
**Hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mengapa ufolens.com dibangun seperti ini

Catatan tentang tiga keputusan yang membentuk [ufolens.com](https://www.ufolens.com) (pembangunan ulang [arsip PURSUE UAP](https://www.war.gov/ufo) yang dapat dicari dan multibahasa). Komentar / sanggahan diterima.

### 1. Pipeline adalah mesin keadaan maju-saja — dengan sengaja

Keadaan: `ditemukan → diunduh → ocr_selesai → diterjemahkan → dipublikasikan`. Sebuah dokumen hanya bergerak maju, dan hanya ketika ada pekerjaan yang harus dilakukan. Konten yang dipublikasikan tidak pernah diproses ulang kecuali detektor delta melihat sumbernya benar-benar berubah.

**Mengapa:** OCR + terjemahan adalah operasi yang mahal, dan arsip tumbuh seiring waktu. Pipeline yang "menjalankan ulang semuanya untuk amannya" memiliki biaya tak terbatas. Membuat transisi mundur tidak mungkin membuat tagihan yang tak terkendali menjadi tidak mungkin. Batas atas biaya adalah properti dari grafik keadaan, bukan dari kewaspadaan operator.

**Biaya:** migrasi skema dan pemrosesan ulang yang disengaja sengaja dibuat canggung. Pertukaran yang dapat diterima.

### 2. OCR dan terjemahan berjalan di LLM lokal, bukan API cloud

OCR: mesin sumber terbuka, fallback Tesseract CLI. Terjemahan + NER: Gemma via Ollama, di laptop Apple Silicon.

**Mengapa:** biaya marjinal nol per dokumen; dapat direproduksi (model + prompt tetap); dan langkah pengambilan sudah harus berjalan dari IP residensial (sumbernya ada di belakang Akamai Bot Manager — `curl` mendapat 403), jadi laptop tetap ada dalam lingkaran.

**Biaya:** kualitas terjemahan di bawah model perbatasan. Untuk korpus referensi di mana bahasa Inggris asli selalu dapat diakses dengan satu klik, itu tidak masalah. Kami tidak mengklaim terjemahan itu otoritatif.

### 3. Dua bagian berbagi tepat satu antarmuka: bundel yang dipublikasikan

Pipeline tidak pernah menulis ke basis data produksi secara langsung. Ia mengeluarkan `{ SQL, manifes aset, daftar pembersihan-cache }`. "Mempublikasikan" = menerapkan bundel itu ke depan (dorong SQL ke DB SQL tepi, sinkronkan aset ke penyimpanan objek, bersihkan kunci cache yang disebutkan).

**Mengapa:** sisi lokal dan sisi tepi dapat berevolusi secara independen; bundel dapat ditinjau; dan "data penerapan" memiliki bentuk yang sama setiap saat. Worker adalah aplikasi TypeScript/Hono kecil — CSP ketat (tanpa `unsafe-inline`; JSON-LD inline di-pin sha256), negosiasi `Accept-Language` + negara→bahasa, cache halaman KV 30 hari, cron pemeliharaan harian — dan tidak pernah perlu tahu bagaimana data itu dibuat.

**Biaya:** perubahan skema D1 menyentuh dua file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Asuransi murah.

### Hal-hal yang tidak bisa ditawar yang tertanam dalam perilaku

- Tidak berafiliasi dengan pemerintah AS; tidak ada lencana resmi.
- Redaksi sumber dipertahankan, tidak pernah dibalikkan.
- Video diatribusikan ke DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` di seluruh situs — dapat diindeks pencarian, memilih keluar dari pengambilan AI.

Langsung: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

