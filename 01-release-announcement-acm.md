# GitHub — منشور 1 من 3 · إعلان الإصدار / فقرة README

**الاستخدام:** كنص أساسي لإصدار على GitHub، أو مناقشة مثبتة، أو في بداية ملف README الخاص بالمستودع.
**الكلمات المفتاحية:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**روابط تشعبية:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — منصة متعددة اللغات وقابلة للبحث لأرشيف PURSUE UAP

**الرابط المباشر:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **الأرشيف المصدر:** https://www.war.gov/ufo

`ufolens.com` يعيد نشر أرشيف **PURSUE** التابع لوزارة الحرب الأمريكية للسجلات المرفوع عنها السرية عن الظواهر الجوية غير المحددة (UAP) / الأجسام الطائرة المجهولة (UFO) كمنصة معرفية: بحث بالنص الكامل، ترجمة آلية عبر المجموعة الكاملة، استكشاف الخرائط والخط الزمني، وواجهة برمجة تطبيقات عامة بصيغة JSON. الوثائق المصدر هي من أعمال الحكومة الفيدرالية للولايات المتحدة وتقع ضمن الملكية العامة داخل الولايات المتحدة ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). هذا المشروع **لا ينتمي إلى حكومة الولايات المتحدة**، ولا يستخدم أي شعارات رسمية، ولا يقوم أبداً بإلغاء التنقيحات.

### الهيكلية

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

- **تكلفة صفرية لكل وثيقة على الذكاء الاصطناعي السحابي.** يتم تشغيل التعرف الضوئي على الحروف (OCR) والترجمة محلياً؛ وتضمن آلية الحالة التقدمية فقط (`مكتشف ← محمل ← تم التعرف الضوئي ← مترجم ← منشور`) عدم إعادة معالجة أي وثيقة ما لم تتغير.
- **جوهر مسار المعالجة (pipeline) لا يعتمد على أي طرف ثالث** — وحدات التحليل / البيان / التغييرات تعمل وتُختبر على بيئة Python نظيفة بدون أي شيء مثبت عبر pip؛ وتتدهور مراحل التعرف الضوئي على الحروف/الترجمة بشكل مقبول عند غياب الحزم الاختيارية.
- **موقع الحافة (Edge site)** يطبق ترويسات أمان صارمة + CSP (بدون `unsafe-inline`؛ JSON-LD المضمن مثبت عبر sha256)، والتفاوض على اللغة عبر `Accept-Language` + ربط البلدان، وذاكرة تخزين مؤقت KV للصفحات لمدة 30 يوماً، ومهمة صيانة يومية مجدولة.
- **تحديثات تزايدية:** يقوم كاشف التغييرات بمقارنة الفهرس المصدر ويغذي مسار المعالجة بالتغييرات فقط.

### للمطورين

توفر واجهة برمجة التطبيقات العامة على https://www.ufolens.com/api/v1 الوثائق والبيانات الوصفية بصيغة JSON. الوصول المجهول محدود بمعدل معين؛ اطلب مفتاحًا للوصول إلى مستويات الباحثين/المطورين. انظر قسم API على الموقع للحصول على نقاط النهاية والحدود.

### الحالة

الكود مكتمل؛ الموقع منشور على https://www.ufolens.com. يتم ملء قاعدة بيانات الإنتاج عن طريق تشغيل مسار المعالجة غير المتصل بالإنترنت ونشر الحزمة (`cli_publish run --remote`). توجد مستندات التصميم الكاملة في `docs/20260511/`.

### الترخيص / الحدود

- الوثائق المصدر: من أعمال الحكومة الفيدرالية للولايات المتحدة، ملكية عامة داخل الولايات المتحدة.
- الكود الخاص بهذه المنصة: انظر `LICENSE`.
- يرسل الموقع `Tdm-Reservation: 1` و `X-Robots-Tag: noai, noimageai` — قابل للفهرسة بواسطة محركات البحث، ولكنه معطل عن التدريب/الكشط بواسطة الذكاء الاصطناعي.
- لقطات الفيديو منسوبة إلى DVIDS / AARO ولا يدعي هذا المشروع ملكيتها.

نرحب بالمشاكل (Issues) وطلبات السحب (PRs). يرجى قراءة `CLAUDE.md` و `docs/20260511/00-*` قبل فتح تغييرات هيكلية.
