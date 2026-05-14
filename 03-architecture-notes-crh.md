# GitHub — 3-ünci yazı (3-ten) · Mimariy qaydlar (ADR-usulı Muzakere)

**Qullanım:** "Körset ve ayt" / "Mimariy" altında bir Muzakere ya da `docs/` ADR tamırı olaraq.
**Açar süzler:** mimariy, ADR, tek ileri areket etken vaziyet maşnası, yerli LLM, Ollama, OCR, kenar esaplav, CSP, telükesizlik üstbilgileri, malümat boru hattı, masraf müendisligi, SQLite manifesti, D1, R2, KV
**Giperbağlantılar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com niçün bu şekilde quruldı

[PURSUE UAP arhiviniñ](https://www.war.gov/ufo) qıdıruv imkânı olğan, çoq tilli yañıdan qurulması olğan [ufolens.com](https://www.ufolens.com)'nı şekillendiren üç qarar aqqında qaydlar. Fikirler / tenqitler qabul etile.

### 1. Boru hattı, maqsatlı olaraq, tek ileri areket etken bir vaziyet maşnasıdır

Vaziyetler: `discovered → downloaded → ocr_done → translated → published`. Bir vesiqa tek ileri areket ete ve tek yapılması kerekken iş olğanda. Neşir etilgen muhteva, bir delta detektoru asılnıñ kerçekten deñişkenini körmese, iç bir vaqıt yañıdan işlenmey.

**Niçün:** OCR + tercime pahalı operatsiyalardır ve arhiv vaqıt ile büyüye. "Emin olmak içün er şeyni yañıdan çalıştırğan" bir boru hattınıñ sıñırsız masrafı bar. Keri keçişlerni imkânsız yapmaq, qontrolsız bir faturanı imkânsız kıla. Masrafnıñ üst sıñırı, operatornıñ diqqatına degil, vaziyet grafiginiñ bir hususiyetidir.

**Bedeli:** şema migratsiyaları ve maqsatlı yañıdan işlevler bilerekten qolaysızdır. Qabul etilir bir deñiş-tokuştır.

### 2. OCR ve tercime, bir bulut API'si degil de, yerli bir LLM üzerinde çalışa

OCR: açıq kodlu motor, Tesseract CLI yedegi. Tercime + NER: Ollama vastasınen Gemma, bir Apple Silicon noutbukında.

**Niçün:** vesiqa başına sıfır marjinal masraf; tekrarlanabilir (sabit model + promtlar); ve aluv adımı endi bir mesken IP'sinden çalışmalı (menba Akamai Bot Manager artında — `curl` 403 ala), bu yüzden bir noutbuk zaten devrede.

**Bedeli:** tercime keyfiyeti, eñ yañı modelniñ astındadır. Asıl İngilizceniñ er vaqıt bir tıklama uzaqlıqta olğanı bir referans korpusı içün, bu normaldır. Tercimelerniñ avtoritetli olğanını iddia etmeymiz.

### 3. Eki yarım, tam olaraq bir interfeysni paylaşa: neşir etilgen bir paket

Boru hattı iç bir vaqıt doğrudan imalât malümat bazasına yazmay. O, `{ SQL, varlıq manifesti, keş temizlev cedveli }` çıqara. "Neşir etmek" = bu paketi ileri qullanmaq (SQL'nı kenar SQL DB'sine itmek, varlıqlarnı obyekt saqlavına sinxronlamaq, adlandırılğan keş anahtarlarını temizlemek).

**Niçün:** yerli taraf ve kenar taraf mustaqil olaraq inkişaf ete bilir; paket közden keçirilebilir; ve "malümat yerleştirmek" er defa aynı şekilde olur. Worker, kiçik bir TypeScript/Hono uygulamasıdır — qattı CSP (`unsafe-inline` yoq; satır içi JSON-LD sha256-belgilengen), `Accept-Language` + ülke→til muzakeresi, 30 künlük KV saife keşi, kündelik baqım cron'ı — ve malümatnıñ nasıl yapılğanını bilmesine iç bir vaqıt ihtiyacı yoq.

**Bedeli:** bir D1 şema deñişikligi eki faylğa tesir ete (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Ucuz bir sigorta.

### Davranışqa siñdirilgen, muzakere etilmeycek şeyler

- ABD ükümetinen bağlı degil; resmiy timsal yoq.
- Asıl redaktsiyalar saqlana, iç bir vaqıt keri çevrilmey.
- Video DVIDS / AARO'ğa aittir.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` bütün saytta — qıdıruvda indekslenir, AI qazımasından vazgeçilgen.

Canlı: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

