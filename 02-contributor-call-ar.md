# GitHub — المنشور 2 من 3 · دعوة للمساهمين / "issues أولى جيدة"

**الاستخدام:** كمناقشة مثبتة ("المساهمة و issues أولى جيدة") أو كمقدمة لملف CONTRIBUTING.md.
**الكلمات المفتاحية:** المصادر المفتوحة، المساهمة، good first issue، i18n، التوطين، OCR، Python، TypeScript، Vitest، pytest، إمكانية الوصول، UAP، البيانات المفتوحة
**الروابط التشعبية:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## المساهمة في ufolens.com

يحول [ufolens.com](https://www.ufolens.com) [PURSUE UAP أرشيف](https://www.war.gov/ufo) وزارة الحرب الأمريكية إلى منصة متعددة اللغات وقابلة للبحث مع [ API عام](https://www.ufolens.com/api/v1). يتكون المشروع من جزأين — مسار استيعاب Python محلي (`pipeline/`) وتطبيق حافة (edge app) TypeScript/Hono (`worker/`) — يلتقيان في واجهة واحدة: حزمة SQL وأصول منشورة.

لا تحتاج إلى أي بيانات اعتماد سحابية للمساهمة. الوحدات الأساسية للمسار تعتمد فقط على المكتبة القياسية (stdlib)، وتعمل اختبارات الـ Worker مقابل تخزين في الذاكرة (in-memory storage).

### الإعداد

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### المجالات التي تكون فيها المساعدة أكثر فائدة

**i18n / التوطين (localization)** — `worker/src/i18n/ui-strings.json` هو مصدر نصوص واجهة المستخدم. مراجعة المتحدثين الأصليين لأي لغة غير الإنجليزية ذات قيمة عالية: رصد المخرجات الآلية الركيكة، وإصلاح RTL/تنسيق issues، وتحسين الحالات الاستثنائية في التفاوض على اللغة (language-negotiation).

**جودة OCR** — تحسين المعالجة المسبقة للمسوحات الضوئية القديمة المكتوبة آلياً قبل OCR؛ وبناء إطار تقييم يقارن بين المحرك مفتوح المصدر والبديل Tesseract على صفحات عينة.

**إمكانية الوصول (Accessibility)** — تدقيق الصفحات المعروضة (`worker/src/render/`) وفقاً لـ WCAG؛ علماً بأن CSP صارم (لا يسمح بـ `unsafe-inline`)، لذا يجب أن تعمل الحلول ضمن هذا الإطار.

**سهولة الاستخدام API (ergonomics)** — `worker/src/routes/` — تقسيم الصفحات، التصفية، وصف OpenAPI، وعملاء أمثلة.

**متانة مسار العمل (Pipeline robustness)** — توفير مسارات تدهور تدريجي (graceful-degradation) أكثر سلاسة، وتحسين تقارير التقدم، ومعالجة الحالات الاستثنائية في اكتشاف الفروقات (delta-detection) (`pipeline/lib/delta.py`).

**الوثائق (Docs)** — `docs/20260511/` (繁體中文؛ `00-*` هو الفهرس). نرحب بترجمات وثائق التصميم إلى اللغة الإنجليزية.

### القواعد الأساسية

- يجب أن تكون جميع المسارات النسبية — المشروع قابلة للنقل بين الأجهزة. لا تستخدم مسارات مطلقة مكتوبة بشكل ثابت (hardcoded).
- لا تضف تبعية pip إلى وحدة *أساسية* في مسار العمل. يمكن للمراحل الاختيارية استخدام حزم اختيارية، ويجب أن تعمل بشكل محدود دون تعطل في حال غيابها.
- لا تضعف آلة الحالة التي تتحرك للأمام فقط (forward-only state machine) — لأنها تمثل سقف التكلفة.
- لا تضف أي شعارات رسمية للحكومة الأمريكية، ولا تضف أي شيء يلغي عمليات التظليل/الحجب (redactions) في المصدر.
- تغييرات مخطط D1 (D1 schema) تؤثر على **ملفين**: `pipeline/lib/manifest_schema.sql` و `db/schema.sql`.
- أرفق الاختبارات مع الكود الجديد. استخدم رسائل التزام (commit) معيارية (Conventional-commit).

اقرأ `CLAUDE.md` و `docs/20260511/00-*` أولاً، ثم افتح issue لمناقشة أي جوانب هيكلية قبل PR.