# GitHub — 3 tadan 2-chi post · Hissa qoʻshishga chaqiruv / "yaxshi birinchi masalalar"

**Qoʻllash:** mahkamlangan Muhokama ("Hissa qoʻshish va yaxshi birinchi masalalar") yoki CONTRIBUTING.md kirish qismi sifatida.
**Kalit soʻzlar:** ochiq manba, hissa qoʻshish, yaxshi birinchi masala, i18n, mahalliylashtirish, OCR, Python, TypeScript, Vitest, pytest, maxsus imkoniyatlar, UAP, ochiq maʼlumotlar
**Giperhavolalar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com ga hissa qoʻshish

[ufolens.com](https://www.ufolens.com) AQSh Harbiy Departamentining [PURSUE UAP arxivini](https://www.war.gov/ufo) qidirish mumkin boʻlgan, koʻp tilli platformaga aylantiradi va [ochiq API](https://www.ufolens.com/api/v1) ga ega. U ikkita qismdan iborat — mahalliy Python qabul qilish konveyeri (`pipeline/`) va TypeScript/Hono chekka ilovasi (`worker/`) — bitta interfeysda uchrashadi: nashr etilgan SQL + aktivlar toʻplami.

Hissa qoʻshish uchun sizga hech qanday bulutli hisob maʼlumotlari kerak emas. Konveyerning asosiy modullari faqat stdlib'dan iborat va Worker testlari xotiradagi omborga qarshi ishlaydi.

### Sozlash

```bash
# konveyer
python3 -m pytest pipeline/tests/          # hammasi yashil boʻlishi kerak, pip oʻrnatish kerak emas

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Yordam eng foydali boʻladigan joylar

**i18n / mahalliylashtirish** — `worker/src/i18n/ui-strings.json` UI satrlarining manbai hisoblanadi. Har qanday ingliz tilidan boshqa tilning ona tilida soʻzlashuvchi tomonidan koʻrib chiqilishi yuqori qiymatga ega: noqulay mashina natijalarini topish, RTL/maket muammolarini tuzatish, til muzokaralaridagi chekka holatlarni yaxshilash.

**OCR sifati** — OCR dan oldin eski yozuv mashinasida yozilgan skanerlarni yaxshiroq oldindan ishlash; ochiq manbali dvigatelni Tesseract zaxira varianti bilan namunaviy sahifalarda solishtirish uchun baholash tizimi.

**Maxsus imkoniyatlar** — render qilingan sahifalarni (`worker/src/render/`) WCAG ga muvofiqligini tekshirish; CSP qatʼiy (`unsafe-inline` yoʻq), shuning uchun yechimlar shu doirada ishlashi kerak.

**API ergonomikasi** — `worker/src/routes/` — sahifalarga boʻlish, filtrlash, OpenAPI tavsifi, misol mijozlar.

**Konveyerning mustahkamligi** — yanada muammosiz ishlaydigan yoʻllar, yaxshiroq taraqqiyot hisoboti, delta-aniqlashning chekka holatlari (`pipeline/lib/delta.py`).

**Hujjatlar** — `docs/20260511/` (繁體中文; `00-*` indeks). Dizayn hujjatlarining ingliz tiliga tarjimalari qabul qilinadi.

### Asosiy qoidalar

- Barcha yoʻllar nisbiy — loyiha mashinalar oʻrtasida koʻchiriladigan boʻlishi kerak. Qattiq kodlangan mutlaq yoʻllar yoʻq.
- Konveyerning *asosiy* moduliga pip bogʻliqligini qoʻshmang. Ixtiyoriy bosqichlar ixtiyoriy paketlardan foydalanishi mumkin va ularsiz muammosiz ishlashi kerak.
- Faqat oldinga harakatlanadigan holat mashinasini zaiflashtirmang — bu xarajatlarning yuqori chegarasi.
- AQSh hukumatining rasmiy belgilarini kiritmang va manbadagi tahrirlarni bekor qiladigan hech narsa qoʻshmang.
- D1 sxemasidagi oʻzgarishlar **ikki** faylga taʼsir qiladi: `pipeline/lib/manifest_schema.sql` va `db/schema.sql`.
- Yangi kod bilan testlar. Conventional-commit xabarlari.

PR'dan oldin har qanday strukturaviy oʻzgarishlarni muhokama qilish uchun avval `CLAUDE.md` va `docs/20260511/00-*` ni oʻqib chiqing, soʻngra masala oching.

