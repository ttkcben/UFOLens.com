# GitHub — ልጥፍ 1 ከ 3 · የሪሊስ / README ማስታወቂያ ክፍል

**ለመጠቀም:** ለ GitHub Release አካል፣ ለተሰካ ውይይት፣ ወይም ለ repo README የላይኛው ክፍል።
**ቁልፍ ቃላት:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**ሃይፐርሊንኮች:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — ለ PURSUE UAP መዝገብ ቤት ብዙ ቋንቋ ተናጋሪ፣ ሊፈለግ የሚችል መድረክ

**ቀጥታ ስርጭት:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **የምንጭ መዝገብ ቤት:** https://www.war.gov/ufo

`ufolens.com` የዩናይትድ ስቴትስ የጦርነት መምሪያን **PURSUE** የተባለውን ከምድብ የወጡ የ UAP / UFO መዝገቦችን እንደ የእውቀት መድረክ በድጋሚ ያትማል፦ ሙሉ-ጽሑፍ ፍለጋ፣ በኮርፐሱ ላይ የማሽን ትርጉም፣ የካርታ + የጊዜ መስመር ፍለጋ፣ እና ይፋዊ የ JSON API ያቀርባል። የምንጭ ሰነዶች የዩናይትድ ስቴትስ ፌዴራል መንግስት ስራዎች ሲሆኑ በአሜሪካ ውስጥ በህዝብ ግዛት ስር ናቸው ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105))። ይህ ፕሮጀክት ከዩናይትድ ስቴትስ መንግስት ጋር **ምንም ግንኙነት የለውም**፣ ምንም አይነት ይፋዊ አርማ አይጠቀምም፣ እና በጭራሽ የተሰረዙ መረጃዎችን አይመልስም።

### አርክቴክቸር

```
የአካባቢ ማሽን (Apple Silicon, የመኖሪያ ቤት IP)       የኤጅ ኔትወርክ
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   ገጾች
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   ይፋዊ API
  translate / NER: local LLM (Gemma via Ollama)        /admin        ኦፕሬተር ኮንሶል
  state: SQLite manifest                             የሚደገፈው በ: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── ያትማል a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **በያንዳንዱ ሰነድ ዜሮ የክላውድ-AI ወጪ።** OCR እና ትርጉም በአካባቢው ይሰራሉ፤ የ forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) ሰነዱ ካልተለወጠ በስተቀር በድጋሚ እንዳይሰራ ያረጋግጣል።
- **የ Pipeline ዋናው ክፍል የሶስተኛ ወገን ጥገኝነት የለውም** — parsing / manifest / delta ሞጁሎች ምንም ነገር pip- ሳይጫን በንጹህ Python ላይ ይሰራሉ እና ይሞከራሉ። OCR/ትርጉም ደረጃዎች አማራጭ ፓኬጆች በሌሉበት ጊዜ በስነስርአት ይቋረጣሉ።
- **የኤጅ ጣቢያው** ጥብቅ የደህንነት ሄደሮችን + CSP ( `unsafe-inline` የለም፤ inline JSON-LD sha256-pinned ነው)፣ በ `Accept-Language` + የሀገር ካርታ አማካኝነት የቋንቋ ድርድር፣ የ30-ቀን KV ገጽ መሸጎጫ፣ እና የዕለት የቤት አያያዝ ክሮን ይተገብራል።
- **ተጨማሪ ዝማኔዎች:** የ delta detector የምንጭ ኢንዴክሱን ይለያል እና ለውጦቹን ብቻ ወደ pipeline መልሶ ይመግባል።

### ለገንቢዎች

በ https://www.ufolens.com/api/v1 ላይ ያለው ይፋዊ API ሰነዶችን እና ሜታዳታን እንደ JSON ይመልሳል። ስም-አልባ መዳረሻ በቁጥር የተገደበ ነው፤ ለተመራማሪ/ገንቢ ደረጃዎች ቁልፍ ይጠይቁ። በጣቢያው ላይ ያለውን የ API ክፍል ለኤንድፖይንቶች እና ገደቦች ይመልከቱ።

### ሁኔታ

ኮድ ተጠናቋል፤ ጣቢያው በ https://www.ufolens.com ላይ ተሰማርቷል። የፕሮዳክሽን ዳታቤዝ ከመስመር ውጭ ያለውን pipeline በማሄድ እና ጥቅሉን ወደ ፊት በማተም (`cli_publish run --remote`) ይሞላል። ሙሉ የንድፍ ሰነዶች በ `docs/20260511/` ውስጥ ይኖራሉ።

### ፈቃድ / ወሰኖች

- የምንጭ ሰነዶች: የዩናይትድ ስቴትስ ፌዴራል መንግስት ስራዎች፣ በአሜሪካ ውስጥ በህዝብ ግዛት ስር።
- የዚህ መድረክ የራሱ ኮድ: `LICENSE` ን ይመልከቱ።
- ጣቢያው `Tdm-Reservation: 1` እና `X-Robots-Tag: noai, noimageai` ይልካል — በፍለጋ ፕሮግራሞች ሊጠቆም ይችላል፣ ከ AI ስልጠና/መቧጨር ወጥቷል።
- የቪዲዮ ምስሎች ለ DVIDS / AARO የተሰጡ ሲሆኑ በዚህ ፕሮጀክት አልተጠየቁም።

ጉዳዮች እና PRs እንኳን ደህና መጡ። መዋቅራዊ ለውጦችን ከመክፈትዎ በፊት እባክዎን `CLAUDE.md` እና `docs/20260511/00-*` ን ያንብቡ።
