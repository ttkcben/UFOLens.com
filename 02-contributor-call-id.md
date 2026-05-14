# GitHub — Kiriman 2 dari 3 · Panggilan kontributor / "isu pertama yang baik"

**Gunakan sebagai:** Diskusi yang disematkan ("Berkontribusi & isu pertama yang baik") atau pengantar CONTRIBUTING.md.
**Kata kunci:** sumber terbuka, berkontribusi, isu pertama yang baik, i18n, lokalisasi, OCR, Python, TypeScript, Vitest, pytest, aksesibilitas, UAP, data terbuka
**Hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Berkontribusi untuk ufolens.com

[ufolens.com](https://www.ufolens.com) mengubah [arsip PURSUE UAP](https://www.war.gov/ufo) dari Departemen Perang AS menjadi platform multibahasa yang dapat dicari dengan [API publik](https://www.ufolens.com/api/v1). Ini terdiri dari dua bagian — pipeline penyerapan Python lokal (`pipeline/`) dan aplikasi tepi TypeScript/Hono (`worker/`) — bertemu di satu antarmuka: bundel SQL + aset yang dipublikasikan.

Anda tidak memerlukan kredensial cloud apa pun untuk berkontribusi. Modul inti pipeline hanya menggunakan stdlib dan tes Worker berjalan terhadap penyimpanan dalam memori.

### Pengaturan

```bash
# pipeline
python3 -m pytest pipeline/tests/          # harusnya semua hijau, tidak perlu pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Di mana bantuan paling bermanfaat

**i18n / lokalisasi** — `worker/src/i18n/ui-strings.json` adalah sumber string UI. Tinjauan penutur asli dari setiap lokal non-Inggris sangat berharga: menangkap hasil mesin yang canggung, memperbaiki masalah RTL/tata letak, meningkatkan kasus-kasus batas negosiasi-bahasa.

**Kualitas OCR** — pra-pemrosesan yang lebih baik untuk pindaian ketik lama sebelum OCR; kerangka evaluasi yang membandingkan mesin sumber terbuka vs. fallback Tesseract pada halaman sampel.

**Aksesibilitas** — audit halaman yang dirender (`worker/src/render/`) terhadap WCAG; CSP-nya ketat (tanpa `unsafe-inline`), jadi solusi harus bekerja di dalamnya.

**Ergonomi API** — `worker/src/routes/` — paginasi, pemfilteran, deskripsi OpenAPI, klien contoh.

**Ketahanan pipeline** — jalur degradasi-anggun yang lebih banyak, pelaporan kemajuan yang lebih baik, kasus-kasus batas deteksi-delta (`pipeline/lib/delta.py`).

**Dokumen** — `docs/20260511/` (繁體中文; `00-*` adalah indeksnya). Terjemahan dokumen desain ke bahasa Inggris diterima.

### Aturan dasar

- Semua jalur bersifat relatif — proyek harus portabel di seluruh mesin. Tidak ada jalur absolut yang di-hardcode.
- Jangan tambahkan dependensi pip ke modul *inti* pipeline. Tahap opsional dapat menggunakan paket opsional, dan harus menurun secara anggun tanpanya.
- Jangan melemahkan mesin keadaan maju-saja — itulah batas biayanya.
- Jangan memperkenalkan lencana resmi pemerintah AS, dan jangan menambahkan apa pun yang membalikkan redaksi sumber.
- Perubahan skema D1 menyentuh **dua** file: `pipeline/lib/manifest_schema.sql` dan `db/schema.sql`.
- Tes dengan kode baru. Pesan conventional-commit.

Baca `CLAUDE.md` dan `docs/20260511/00-*` terlebih dahulu, lalu buka isu untuk mendiskusikan apa pun yang bersifat struktural sebelum PR.

