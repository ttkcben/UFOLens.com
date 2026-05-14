# GitHub — 3 tadan 3-chi post · Arxitektura qaydlari (ADR uslubidagi muhokama)

**Qoʻllash:** "Koʻrsatish va aytib berish" / "Arxitektura" ostidagi Muhokama yoki `docs/` ADR asosi sifatida.
**Kalit soʻzlar:** arxitektura, ADR, faqat oldinga harakatlanadigan holat mashinasi, mahalliy LLM, Ollama, OCR, chekka hisoblash, CSP, xavfsizlik sarlavhalari, maʼlumotlar konveyeri, xarajatlarni boshqarish, SQLite manifesti, D1, R2, KV
**Giperhavolalar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Nima uchun ufolens.com aynan shu tarzda qurilgan

[ufolens.com](https://www.ufolens.com) ([PURSUE UAP arxivining](https://www.war.gov/ufo) qidirish mumkin boʻlgan, koʻp tilli qayta qurilishi) ni shakllantirgan uchta qaror haqida qaydlar. Fikr-mulohazalar / eʼtirozlar qabul qilinadi.

### 1. Konveyer — bu ataylab faqat oldinga harakatlanadigan holat mashinasi

Holatlar: `topildi → yuklab olindi → ocr_tugallandi → tarjima qilindi → nashr etildi`. Hujjat faqat oldinga siljiydi va faqat bajariladigan ish boʻlganda. Nashr etilgan kontent, agar delta detektori manba haqiqatan ham oʻzgarganini koʻrmasa, hech qachon qayta ishlanmaydi.

**Nima uchun:** OCR + tarjima qimmat operatsiyalardir va arxiv vaqt oʻtishi bilan oʻsib boradi. "Xavfsizlik uchun hamma narsani qayta ishga tushiradigan" konveyer cheksiz xarajatlarga ega. Orqaga oʻtishni imkonsiz qilish, nazoratsiz xarajatlarni imkonsiz qiladi. Xarajatlarning yuqori chegarasi operatorning hushyorligiga emas, balki holat grafigining xususiyatidir.

**Narxi:** sxema migratsiyalari va ataylab qayta ishlash qasddan noqulay qilingan. Qabul qilinadigan kelishuv.

### 2. OCR va tarjima bulutli API da emas, balki mahalliy LLM da ishlaydi

OCR: ochiq manbali dvigatel, Tesseract CLI zaxira varianti. Tarjima + NER: Gemma orqali Ollama, Apple Silicon noutbukida.

**Nima uchun:** har bir hujjat uchun nolga teng chekka xarajat; takrorlanuvchanlik (belgilangan model + soʻrovlar); va yuklash bosqichi allaqachon turar-joy IP'sidan ishlashi kerak (manba Akamai Bot Manager orqasida — `curl` 403 oladi), shuning uchun noutbuk baribir jarayonda ishtirok etadi.

**Narxi:** tarjima sifati eng ilgʻor modeldan pastroq. Asl inglizcha versiyasi har doim bir marta bosish masofasida boʻlgan maʼlumotnoma korpusi uchun bu maqbuldir. Biz tarjimalarning rasmiy ekanligini daʼvo qilmaymiz.

### 3. Ikkala qism ham faqat bitta interfeysni boʻlishadi: nashr etilgan toʻplam

Konveyer hech qachon toʻgʻridan-toʻgʻri ishlab chiqarish maʼlumotlar bazasiga yozmaydi. U `{ SQL, aktivlar manifesti, keshni tozalash roʻyxati }` ni chiqaradi. "Nashr etish" = ushbu toʻplamni oldinga qoʻllash (SQL'ni chekka SQL DB'ga yuborish, aktivlarni obyekt omboriga sinxronlash, nomlangan kesh kalitlarini tozalash).

**Nima uchun:** mahalliy va chekka tomonlar mustaqil ravishda rivojlana oladi; toʻplamni koʻrib chiqish mumkin; va "maʼlumotlarni joylashtirish" har doim bir xil shaklga ega. Worker kichik bir TypeScript/Hono ilovasi — qatʼiy CSP ( `unsafe-inline` yoʻq; ichki JSON-LD sha256-mahkamlangan), `Accept-Language` + mamlakat→til muzokaralari, 30 kunlik KV sahifa keshi, kunlik tozalash cron'i — va u hech qachon maʼlumotlar qanday yaratilganini bilishi shart emas.

**Narxi:** D1 sxemasidagi oʻzgarish ikki faylga taʼsir qiladi (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Arzon sugʻurta.

### Xulq-atvorga singdirilgan, muhokama qilinmaydigan qoidalar

- AQSh hukumati bilan bogʻliq emas; rasmiy belgilar yoʻq.
- Manbadagi tahrirlar saqlanadi, hech qachon bekor qilinmaydi.
- Video DVIDS / AARO ga tegishli.
- Butun sayt boʻylab `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` — qidiruvda indekslanadigan, AI tomonidan qirib olinishdan voz kechilgan.

Jonli efirda: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
