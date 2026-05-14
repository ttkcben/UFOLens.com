# GitHub — المنشور 1 من 3 · إعلان الإصدار / قسم README

**الاستخدام:** كنص لإصدار على GitHub، أو مناقشة مثبتة، أو في بداية ملف README للمستودع.
**الكلمات المفتاحية:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**روابط:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — منصة متعددة اللغات وقابلة للبحث لأرشيف PURSUE الخاص بالـ UAP

**الموقع المباشر:** https://www.ufolens.com  ·  **واجهة برمجة التطبيقات (API):** https://www.ufolens.com/api/v1  ·  **الأرشيف المصدر:** https://www.war.gov/ufo

يعيد `ufolens.com` نشر أرشيف **PURSUE** التابع لوزارة الحرب الأمريكية، والذي يحتوي على سجلات رفعت عنها السرية حول الظواهر الجوية غير المبررة (UAP) / الأجسام الطائرة المجهولة (UFO)، كمنصة معرفية: بحث نصي كامل، ترجمة آلية للمجموعة الكاملة، استكشاف عبر الخرائط والخطوط الزمنية، وواجهة برمجة تطبيقات عامة بصيغة JSON. المستندات المصدر هي من أعمال الحكومة الفيدرالية الأمريكية وتعتبر ضمن الملكية العامة داخل الولايات المتحدة ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). هذا المشروع **لا يتبع للحكومة الأمريكية**، ولا يستخدم أي شعارات رسمية، ولا يقوم أبداً بإلغاء التنقيحات الموجودة في المستندات.

### البنية الهيكلية

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

- **تكلفة صفر لكل مستند على الذكاء الاصطناعي السحابي.** تتم عمليات OCR والترجمة محليًا؛ آلية الحالة ذات المسار الأمامي الإجباري (`discovered → downloaded → ocr_done → translated → published`) تضمن عدم إعادة معالجة أي مستند إلا إذا تغير.
- **الجزء الأساسي من خط الأنابيب لا يعتمد على أي مكتبات خارجية** — وحدات التحليل / البيان / الدلتا تعمل وتُختبر على بيئة Python نظيفة بدون تثبيت أي شيء عبر pip؛ مراحل OCR/الترجمة تتكيف برشاقة عند غياب الحزم الاختيارية.
- **تطبيق الحافة** يطبق ترويسات أمان صارمة + CSP (بدون `unsafe-inline`؛ JSON-LD المضمن مثبت بـ sha256)، التفاوض على اللغة عبر `Accept-Language` + ربط البلدان، ذاكرة تخزين مؤقت للصفحات (KV cache) لمدة 30 يومًا، ومهمة صيانة يومية مجدولة (cron).
- **تحديثات تزايدية:** كاشف التغييرات (delta detector) يقارن فهرس المصدر ويزود خط الأنابيب بالتغييرات فقط.

### للمطورين

توفر واجهة برمجة التطبيقات العامة على https://www.ufolens.com/api/v1 المستندات والبيانات الوصفية بصيغة JSON. الوصول المجهول محدود بمعدل معين؛ اطلب مفتاحًا للوصول بمستويات الباحثين/المطورين. راجع قسم API على الموقع لمعرفة النقاط النهائية والحدود.

### الحالة

الكود مكتمل؛ الموقع منشور على https://www.ufolens.com. يتم ملء قاعدة البيانات الإنتاجية عن طريق تشغيل خط الأنابيب غير المتصل بالإنترنت ونشر الحزمة (`cli_publish run --remote`). وثائق التصميم الكاملة موجودة في `docs/20260511/`.

### الترخيص / الحدود

- المستندات المصدر: من أعمال الحكومة الفيدرالية الأمريكية، ملكية عامة داخل الولايات المتحدة.
- كود هذه المنصة الخاص: انظر `LICENSE`.
- يرسل الموقع `Tdm-Reservation: 1` و `X-Robots-Tag: noai, noimageai` — قابل للفهرسة من قبل محركات البحث، مع إلغاء الاشتراك من استخدامه في تدريب الذكاء الاصطناعي/الكشط.
- لقطات الفيديو منسوبة إلى DVIDS / AARO ولا يدعي هذا المشروع ملكيتها.

نرحب بالقضايا (Issues) وطلبات السحب (PRs). يرجى قراءة `CLAUDE.md` و `docs/20260511/00-*` قبل فتح تغييرات هيكلية.

