# GitHub — Siaran 2 daripada 3 · Panggilan penyumbang / "isu pertama yang baik"

**Guna sebagai:** Perbincangan yang disemat ("Menyumbang & isu pertama yang baik") atau pengenalan CONTRIBUTING.md.
**Kata kunci:** sumber terbuka, menyumbang, isu pertama yang baik, i18n, penyetempatan, OCR, Python, TypeScript, Vitest, pytest, kebolehcapaian, UAP, data terbuka
**Hiperpautan:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Menyumbang kepada ufolens.com

[ufolens.com](https://www.ufolens.com) mengubah [arkib PURSUE UAP](https://www.war.gov/ufo) Jabatan Perang A.S. menjadi platform boleh dicari dan berbilang bahasa dengan [API awam](https://www.ufolens.com/api/v1). Ia terdiri daripada dua bahagian — pipeline pengambilan Python tempatan (`pipeline/`) dan aplikasi pinggir TypeScript/Hono (`worker/`) — bertemu pada satu antara muka: berkas SQL + aset yang diterbitkan.

Anda tidak memerlukan sebarang kelayakan awan untuk menyumbang. Modul teras pipeline adalah stdlib-sahaja dan ujian Worker berjalan terhadap storan dalam memori.

### Persediaan

```bash
# pipeline
python3 -m pytest pipeline/tests/          # sepatutnya semua hijau, tiada pemasangan pip diperlukan

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Di mana bantuan paling berguna

**i18n / penyetempatan** — `worker/src/i18n/ui-strings.json` adalah sumber rentetan UI. Semakan oleh penutur asli bagi mana-mana lokaliti bukan Inggeris adalah sangat berharga: kesan output mesin yang janggal, perbaiki isu RTL/tata letak, perbaiki kes pinggir perundingan bahasa.

**Kualiti OCR** — pra-pemprosesan yang lebih baik bagi imbasan bertaip lama sebelum OCR; abah-abah penilaian membandingkan enjin sumber terbuka dengan sandaran Tesseract pada halaman sampel.

**Kebolehcapaian** — audit halaman yang dipaparkan (`worker/src/render/`) terhadap WCAG; CSP adalah ketat (tiada `unsafe-inline`), jadi penyelesaian mesti berfungsi di dalamnya.

**Ergonomik API** — `worker/src/routes/` — penomboran halaman, penapisan, perihalan OpenAPI, pelanggan contoh.

**Keteguhan pipeline** — lebih banyak laluan penurunan taraf yang baik, pelaporan kemajuan yang lebih baik, kes pinggir pengesanan delta (`pipeline/lib/delta.py`).

**Dokumentasi** — `docs/20260511/` (繁體中文; `00-*` adalah indeks). Terjemahan dokumen reka bentuk ke Bahasa Inggeris dialu-alukan.

### Peraturan asas

- Semua laluan adalah relatif — projek mesti mudah alih merentasi mesin. Tiada laluan mutlak yang dikodkan keras.
- Jangan tambah kebergantungan pip pada modul *teras* pipeline. Peringkat pilihan boleh menggunakan pakej pilihan, dan mesti menurun taraf dengan baik tanpanya.
- Jangan lemahkan mesin keadaan maju-sahaja — itulah siling kos.
- Jangan perkenalkan lambang rasmi kerajaan A.S., dan jangan tambah apa-apa yang menterbalikkan suntingan sumber.
- Perubahan skema D1 menyentuh **dua** fail: `pipeline/lib/manifest_schema.sql` dan `db/schema.sql`.
- Ujian dengan kod baharu. Mesej komitmen konvensional.

Baca `CLAUDE.md` dan `docs/20260511/00-*` terlebih dahulu, kemudian buka isu untuk membincangkan apa-apa yang berstruktur sebelum PR.

