# GitHub — المنشور 2 من 3 · دعوة للمساهمة / "good first issues"

**استخدام كـ:** مناقشة مثبتة ("المساهمة و good first issues") أو مقدمة لـ CONTRIBUTING.md.
**الكلمات المفتاحية:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**روابط:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## المساهمة ف ufolens.com

[ufolens.com](https://www.ufolens.com) كيحول أرشيف [PURSUE UAP archive](https://www.war.gov/ufo) ديال وزارة الحرب الأمريكية لمنصة متعددة اللغات وقابلة للبحث مع [API عمومية](https://www.ufolens.com/api/v1). هو مقسوم على جوج — pipeline ديال Python محلي للاستيعاب (`pipeline/`) و تطبيق edge بـ TypeScript/Hono (`worker/`) — كيتلاقاو ف واجهة وحدة: حزمة منشورة ديال SQL + assets.

ما تحتاج حتى credentials ديال الـcloud باش تساهم. الوحدات الأساسية ديال الـpipeline هي stdlib-only و الـtests ديال الـWorker خدامين على storage ف الذاكرة.

### الإعداد

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### فين المساعدة مفيدة بزاف

**i18n / localization** — `worker/src/i18n/ui-strings.json` هو المصدر ديال نصوص الواجهة. المراجعة من طرف متحدث أصلي لأي لغة ماشي الإنجليزية عندها قيمة كبيرة: تصحيح المخرجات الآلية الغريبة، إصلاح مشاكل RTL/التخطيط، تحسين الحالات الخاصة فالتفاوض على اللغة.

**جودة الـ OCR** — معالجة مسبقة أفضل للمستندات القديمة المطبوعة بالآلة الكاتبة قبل الـ OCR؛ إطار تقييم كيقارن المحرك مفتوح المصدر مع البديل Tesseract على صفحات عينة.

**الولوجية (Accessibility)** — تدقيق الصفحات المعروضة (`worker/src/render/`) مقابل معايير WCAG؛ الـ CSP صارم (ماكايناش `unsafe-inline`)، لذا الحلول خاصها تخدم فهاد الإطار.

**سهولة استخدام الـ API** — `worker/src/routes/` — التقسيم لصفحات، الفلترة، وصف OpenAPI، أمثلة للـ clients.

**متانة الـ Pipeline** — مسارات أكثر للسلاسة فالتقصير، تقارير تقدم أفضل، حالات خاصة فكشف الفروقات (`pipeline/lib/delta.py`).

**الوثائق (Docs)** — `docs/20260511/` (بالصينية التقليدية `繁體中文`؛ `00-*` هو الفهرس). ترجمة وثائق التصميم للإنجليزية مرحب بها.

### القواعد الأساسية

- كل المسارات نسبية — المشروع خاصو يكون قابل للنقل بين الأجهزة. ممنوع مسارات مطلقة مكتوبة فالكود.
- ماتزيدش تبعية pip لوحدة *أساسية* فالـ pipeline. المراحل الاختيارية تقدر تستعمل packages اختيارية، و خاصها تنقص من الجودة ديالها بشكل سلس بلا بيهم.
- ماتضعفش الـ state machine ذات الاتجاه الواحد للأمام — هداك هو سقف التكلفة.
- ماتزيدش شارات رسمية ديال الحكومة الأمريكية، وماتزيد حتى حاجة كتعكس التنقيحات الأصلية.
- تغييرات D1 schema كتخص **جوج** ملفات: `pipeline/lib/manifest_schema.sql` و `db/schema.sql`.
- Tests مع الكود الجديد. رسائل conventional-commit.

قرا `CLAUDE.md` و `docs/20260511/00-*` هو الأول، من بعد حل issue باش تناقش أي تغيير هيكلي قبل ما دير PR.
