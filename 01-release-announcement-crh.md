# GitHub — 1-inci yazı (3-ten) · Reliz / README ilân bloğı

**Qullanım:** GitHub Reliz gövdesi, sabitleştirilgen bir Muzakere ya da repo README'siniñ üst qısmı.
**Açar süzler:** UAP, UFO, PURSUE arkhivi, sırrı açılğan vesiqalar, açıq malümat, tolu metinli qıdıruv, OCR, maşna tercimesi, yerli LLM, Ollama, kenar esaplav, umumiy API, Hono, TypeScript, Python
**Giperbağlantılar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP arkhivi içün çoq tilli, qıdıruv imkânı olğan bir platforma

**Canlı:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Menba arkhivi:** https://www.war.gov/ufo

`ufolens.com`, ABD Cenkleşüv Departamenti'niñ **PURSUE** arkhivindeki sırrı açılğan UAP / UFO kayıtlarını bir bilgi platforması olaraq kene neşir ete: tolu metinli qıdıruv, korpus boyunca maşna tercimesi, harita + zaman cetveli keşfiyatı ve umumi bir JSON API. Asıl vesiqalar ABD federal ükümetiniñ eserleridir ve ABD içinde cemaat mülkiyeti sayılır ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Bu leyha **ABD ükümetinen bağlı degildir**, resmiy timsaller qullanmay ve iç bir vaqıt redaktsiyalarnı keri çevirmey.

### Mimarisi

```
Yerli maşna (Apple Silicon, mesken IP)             Kenar ağı
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, tek stdlib esaslı özü)     worker/  (TypeScript, Hono.js)
  aluv → OCR → tercime → neşir (tek ileri)             /{lang}/...   saifeler
  OCR: açıq kodlu motor (Tesseract CLI yedegi)         /api/v1/...   umumi API
  tercime / NER: yerli LLM (Ollama vastasınen Gemma)   /admin        operator konsolu
  vaziyet: SQLite manifest                          desteklengenler: kenar SQL DB, obyekt
        │                                             saqlav (asıl PDFler), KV keşi
        └── bir paket neşir ete: SQL + varlıq manifesti + keş temizlev cedveli ──┘
```

- **Vesiqa başına sıfır bulut-AI masrafı.** OCR ve tercime yerli olaraq çalışa; tek ileri areket etken vaziyet maşnası (`keşf etildi → endirildi → ocr_bitti → tercime_etildi → neşir_etildi`) bir vesiqanıñ, o deñişmegen olsa, yañıdan işlenmemesini garantiyley.
- **Boru hattı özüniñ üçünci taraf bağlılıqları yoqtır** — teşkir etüv / manifest / delta modulleri temiz bir Python üzerinde, iç bir şey pip ile qurulmadan çalışa ve test etile; OCR/tercime basamaqları, istege bağlı paketler olmağanda, zarif bir şekilde eksik çalışa.
- **Kenar saytı** qattı telükesizlik üstbilgileri + CSP (`unsafe-inline` yoq; satır içi JSON-LD sha256-belgilengen), `Accept-Language` + ülke eşleştirme vastasınen til muzakeresi, 30 künlük bir KV saife keşi ve kündelik bir baqım cron'ı qullana.
- **Arttırımlı yañartuvlar:** bir delta detektoru asıl indekste farqlılıqlarnı tapa ve tek deñişikliklerni boru hattına keri yollay.

### Programmistler içün

https://www.ufolens.com/api/v1 adresindeki umumi API, vesiqalarnı ve üstverilerni JSON olaraq qaytara. Anonim irişim sıñırlıdır; tedqiqatçı/programmist seviyeleri içün bir anahtar talap etiñiz. End-noktalar ve sıñırlar içün sayttaki API bölügine baqıñız.

### Vaziyeti

Kod tamamlandı; sayt https://www.ufolens.com adresinde yerleştirildi. İmalât malümat bazası, çevrimdışı boru hattını çalıştırıp paketi ileri neşir eterek (`cli_publish run --remote`) toldurıla. Tolu dizayn vesiqaları `docs/20260511/` içinde buluna.

### Litsenziya / sıñırlar

- Asıl vesiqalar: ABD federal ükümetiniñ eserleri, ABD içinde cemaat mülkiyeti.
- Bu platformanıñ öz kodı: `LICENSE` faylına baqıñız.
- Sayt `Tdm-Reservation: 1` ve `X-Robots-Tag: noai, noimageai` yollay — qıdıruv motorları tarafından indekslene bilir, AI oqutuvı/qazımasından vazgeçilgen.
- Video görüntüleri DVIDS / AARO'ğa aittir ve bu leyha tarafından iddia etilmey.

Meseleler ve PR'lar qabul etile. Strukturalı deñişiklikler açmazdan evel `CLAUDE.md` ve `docs/20260511/00-*` fayllarını oquñız.

