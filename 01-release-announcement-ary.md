# GitHub — المنشور 1 من 3 · إعلان الإصدار / بلوك README

**استخدام كـ:** محتوى لـ GitHub Release، مناقشة مثبتة، أو فبداية README ديال الريبو.
**الكلمات المفتاحية:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**روابط:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — منصة متعددة اللغات وقابلة للبحث لأرشيف PURSUE UAP

**خدام دابا:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **الأرشيف الأصلي:** https://www.war.gov/ufo

`ufolens.com` كيعاود ينشر أرشيف **PURSUE** ديال وزارة الحرب الأمريكية الخاص بسجلات UAP / UFO لي تحيدات عليها السرية، كمنصة معرفية: بحث بالنص الكامل، ترجمة آلية ف корпуوس كامل، استكشاف الخرائط والخط الزمني، و API عمومية ب JSON. الوثائق الأصلية هي أعمال ديال الحكومة الفيدرالية الأمريكية و ف الولايات المتحدة كتعتبر ملك عام ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). هاد المشروع **ما عندو حتى علاقة مع الحكومة الأمريكية**، ما كيستعمل حتى شارة رسمية، و أبدا ما كيعكسش التنقيحات.

### البنية

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

- **تكلفة صفرية ديال cloud-AI لكل وثيقة.** الـ OCR والترجمة كيدارو محلياً؛ الـ state machine لي كتمشي غير لقدام (`discovered → downloaded → ocr_done → translated → published`) كتضمن أن حتى وثيقة ما كتعاود تعالج إلا إذا تبدلات.
- **القلب ديال الـpipeline مافيه حتى تبعية لطرف ثالث** — الوحدات ديال الـ parsing / manifest / delta خدامين و كيتيستاو على Python نقي بلا حتى حاجة متبيتة بـ pip؛ المراحل ديال OCR/translation كتنقص من الجودة ديالها بشكل سلس فاش كيكونو الـ packages الاختيارية ماكاينينش.
- **الموقع على الـEdge** كيطبق headers ديال الأمان صارمة + CSP (ماكايناش `unsafe-inline`؛ JSON-LD المضمن كيتثبت بـ sha256)، التفاوض على اللغة عبر `Accept-Language` + ربط البلد، cache ديال الصفحات ف KV لمدة 30 يوم، و cron يومي للصيانة.
- **تحديثات تزايدية:** واحد الكاشف ديال الفروقات (delta detector) كيقارن الفهرس الأصلي و كيدوز غير التغييرات للـ pipeline.

### للمطورين

الـ API العمومية فـ https://www.ufolens.com/api/v1 كترجع الوثائق و الـ metadata بصيغة JSON. الوصول المجهول محدود ف المعدل؛ طلب مفتاح لولوج مستويات الباحثين/المطورين. شوف قسم ה-API فالموقع باش تعرف الـ endpoints و الحدود.

### الحالة

الكود كامل؛ الموقع منشور على https://www.ufolens.com. قاعدة البيانات ديال الإنتاج كتعمر عن طريق تشغيل الـ pipeline أوفلاين ونشر الحزمة لقدام (`cli_publish run --remote`). وثائق التصميم الكاملة كاينة فـ `docs/20260511/`.

### الرخصة / الحدود

- الوثائق الأصلية: أعمال الحكومة الفيدرالية الأمريكية، ملك عام داخل الولايات المتحدة.
- الكود الخاص بهاد المنصة: شوف `LICENSE`.
- الموقع كيرسل `Tdm-Reservation: 1` و `X-Robots-Tag: noai, noimageai` — قابل للفهرسة من طرف محركات البحث، ومختار عدم المشاركة ف تدريب/كشط الذكاء الاصطناعي.
- لقطات الفيديو منسوبة لـ DVIDS / AARO وماكيطالبش بيها هاد المشروع.

مرحبا بالـ Issues و الـ PRs. الرجاء قراءة `CLAUDE.md` و `docs/20260511/00-*` قبل فتح أي تغييرات هيكلية.
