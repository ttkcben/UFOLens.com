# GitHub — المنشور 1 من 3 · كتلة إعلان الإصدار / ملف README

**الاستخدام:** كجسم لإصدار GitHub، أو مناقشة مثبتة (pinned Discussion)، أو في الجزء العلوي من ملف README الخاص بالمستودع.
**الكلمات المفتاحية:** UAP، UFO، أرشيف PURSUE، وثائق رفعت عنها السرية، بيانات مفتوحة، بحث في النص الكامل، OCR، ترجمة آلية، نموذج لغوي كبير محلي (local LLM)، Ollama، حوسبة الحافة (edge computing)، API عام، Hono، TypeScript، Python
**الروابط التشعبية:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — منصة متعددة اللغات وقابلة للبحث لأرشيف PURSUE UAP

**الموقع المباشر:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **الأرشيف المصدري:** https://www.war.gov/ufo

يقوم `ufolens.com` بإعادة نشر أرشيف **PURSUE** التابع لوزارة الحرب الأمريكية للسجلات التي رُفعت عنها السرية UAP / UFO كمنصة معرفية: بحث في النص الكامل، ترجمة آلية عبر المجموعة النصية، استكشاف عبر الخريطة والجدول الزمني، و JSON API عام. الوثائق المصدرية هي أعمال تابعة للحكومة الفيدرالية الأمريكية وهي ضمن الملك العام داخل الولايات المتحدة ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). هذا المشروع **غير تابع للحكومة الأمريكية**، ولا يستخدم أي شعارات رسمية، ولا يقوم أبداً بإلغاء التظليل (redactions) في الوثائق.

### البنية التقنية

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

- **تكلفة صفرية للذكاء الاصطناعي السحابي لكل وثيقة.** تعمل OCR والترجمة محلياً؛ وتضمن آلة الحالة ذات الاتجاه الأمامي فقط (`discovered → downloaded → ocr_done → translated → published`) عدم إعادة معالجة أي وثيقة إلا إذا تغيرت.
- **نواة خط المعالجة (Pipeline) لا تعتمد على أطراف خارجية**؛ حيث تعمل وتُختبر وحدات التحليل (parsing) / البيان (manifest) / التغيرات (delta) — على بيئة Python نظيفة دون تثبيت أي حزم عبر pip؛ وتتراجع مراحل OCR/الترجمة بشكل تدريجي وسلس (degrade gracefully) عند غياب الحزم الاختيارية.
- **موقع الحافة (Edge site)** يطبق رؤوس أمان صارمة + CSP (بدون `unsafe-inline`؛ JSON-LD مدمج ومثبت بـ sha256)، وتفاوض اللغة عبر `Accept-Language` + تعيين الدولة، وذاكرة تخزين مؤقت للصفحات (KV page cache) لمدة 30 يوماً، ومهمة مجدولة (cron) يومية للصيانة.
- **تحديثات تراكمية:** يقوم كاشف التغيرات (delta detector) بمقارنة الفروقات في الفهرس المصدري وتغذية التغييرات فقط مرة أخرى في خط المعالجة.

### للمطورين

توفر API العامة في https://www.ufolens.com/api/v1 الوثائق والبيانات الوصفية (metadata) بصيغة JSON. الوصول المجهول محدود المعدل (rate-limited)؛ يرجى طلب مفتاح للفئات المخصصة للباحثين/المطورين. راجع قسم API على الموقع لمعرفة نقاط النهاية (endpoints) والحدود.

### الحالة

البرمجية مكتملة؛ الموقع منشور في https://www.ufolens.com.. يتم ملء قاعدة بيانات الإنتاج عن طريق تشغيل خط المعالجة غير المتصل بالإنترنت ونشر الحزمة للأمام (`cli_publish run --remote`). تتوفر وثائق التصميم الكاملة في `docs/20260511/`.

### الترخيص / الحدود

- الوثائق المصدرية: أعمال الحكومة الفيدرالية الأمريكية، وهي ملك عام داخل الولايات المتحدة.
- الكود الخاص بهذه المنصة: راجع `LICENSE`.
- يرسل الموقع `Tdm-Reservation: 1` و `X-Robots-Tag: noai, noimageai` — قابلة للفهرسة بواسطة محركات البحث، مع استبعادها من تدريب الذكاء الاصطناعي أو الكشط (scraping).
- تعود ملكية لقطات الفيديو إلى DVIDS / AARO ولا يدعي هذا المشروع ملكيتها.

نرحب بالبلاغات (Issues) و PRs. يرجى قراءة `CLAUDE.md` و `docs/20260511/00-*` قبل اقتراح تغييرات هيكلية.