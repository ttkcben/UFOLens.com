# GitHub — ልጥፍ 2 ከ 3 · የአስተዋጽኦ ጥሪ / "ጥሩ የመጀመሪያ ጉዳዮች"

**ለመጠቀም:** ለተሰካ ውይይት ("ማበርከት እና ጥሩ የመጀመሪያ ጉዳዮች") ወይም ለ CONTRIBUTING.md መግቢያ።
**ቁልፍ ቃላት:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**ሃይፐርሊንኮች:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ለ ufolens.com አስተዋፅኦ ማድረግ

[ufolens.com](https://www.ufolens.com) የዩናይትድ ስቴትስ የጦርነት መምሪያን [PURSUE UAP archive](https://www.war.gov/ufo) ወደሚፈለግ፣ ብዙ ቋንቋ ተናጋሪ መድረክ ከ [public API](https://www.ufolens.com/api/v1) ጋር ይለውጠዋል። እሱ ሁለት ግማሾች አሉት — የአካባቢ Python ingest pipeline (`pipeline/`) እና የ TypeScript/Hono edge app (`worker/`) — በአንድ በይነገጽ ላይ ይገናኛሉ፦ የታተመ የ SQL + assets bundle።

አስተዋፅኦ ለማድረግ ምንም የክላውድ ምስክርነቶች አያስፈልጉዎትም። የ pipeline ዋና ሞጁሎች stdlib-only ሲሆኑ የ Worker ሙከራዎች ከማህደረ ትውስታ ማከማቻ ጋር ይሰራሉ።

### ማዋቀር

```bash
# pipeline
python3 -m pytest pipeline/tests/          # ሁሉም አረንጓዴ መሆን አለበት፣ ምንም pip install አያስፈልግም

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### እርዳታ በጣም ጠቃሚ የሆነባቸው ቦታዎች

**i18n / localization** — `worker/src/i18n/ui-strings.json` የ UI strings ምንጭ ነው። ከእንግሊዝኛ ውጪ ያለ ማንኛውም አካባቢ በአፍ መፍቻ ቋንቋ ተናጋሪ መገምገሙ ከፍተኛ ዋጋ አለው፦ ያልተለመደ የማሽን ውጤትን ይያዙ፣ የ RTL/layout ጉዳዮችን ያስተካክሉ፣ የቋንቋ-ድርድር ጠርዝ ጉዳዮችን ያሻሽሉ።

**OCR ጥራት** — የድሮ የታይፕ የተደረጉ ስካኖችን ከ OCR በፊት የተሻለ ቅድመ-ሂደት፤ የ open-source engineን ከ Tesseract fallback ጋር በናሙና ገጾች ላይ የሚያወዳድር የግምገማ ማሰሪያ።

**ተደራሽነት** — የተሰሩትን ገጾች (`worker/src/render/`) ከ WCAG ጋር ያ аудиት ያድርጉ፤ CSP ጥብቅ ነው ( `unsafe-inline` የለም)፣ ስለዚህ መፍትሄዎች በዚያ ውስጥ መስራት አለባቸው።

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — ይበልጥ ግርማ ሞገስ ያለው-የመበላሸት መንገዶች፣ የተሻለ የእድገት ሪፖርት ማድረግ፣ የ delta-detection ጠርዝ ጉዳዮች (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ኢንዴክስ ነው)። የንድፍ ሰነዶችን ወደ እንግሊዝኛ መተርጎም እንኳን ደህና መጡ።

### መሰረታዊ ህጎች

- ሁሉም መንገዶች አንጻራዊ ናቸው — ፕሮጀክቱ በማሽኖች መካከል ተንቀሳቃሽ መሆን አለበት። ምንም ሃርድ-ኮድ የተደረገ ፍጹም መንገድ የለም።
- ወደ pipeline *core* module የ pip ጥገኝነት አይጨምሩ። አማራጭ ደረጃዎች አማራጭ ፓኬጆችን ሊጠቀሙ ይችላሉ፣ እና ያለነሱ በስነስርአት መበላሸት አለባቸው።
- የ forward-only state machineን አያዳክሙ — ያ የወጪ ጣሪያው ነው።
- የዩናይትድ ስቴትስ መንግስት ይፋዊ አርማ አያስተዋውቁ፣ እና የምንጭ ስረዛዎችን የሚመልስ ምንም ነገር አይጨምሩ።
- የ D1 schema ለውጦች **ሁለት** ፋይሎችን ይነካሉ፦ `pipeline/lib/manifest_schema.sql` እና `db/schema.sql`።
- ከአዲስ ኮድ ጋር ሙከራዎች። Conventional-commit መልዕክቶች።

`CLAUDE.md` እና `docs/20260511/00-*` ን መጀመሪያ ያንብቡ፣ ከዚያ ከ PR በፊት ስለማንኛውም መዋቅራዊ ነገር ለመወያየት አንድ ጉዳይ ይክፈቱ።
