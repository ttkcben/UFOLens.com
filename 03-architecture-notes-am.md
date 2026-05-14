# GitHub — ልጥፍ 3 ከ 3 · የአርክቴክቸር ማስታወሻዎች (ADR-style Discussion)

**ለመጠቀም:** በ "Show and tell" / "Architecture" ስር ለውይይት፣ ወይም ለ `docs/` ADR ዘር።
**ቁልፍ ቃላት:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**ሃይፐርሊንኮች:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com ለምን በዚህ መንገድ እንደተገነባ

[ufolens.com](https://www.ufolens.com) (የ [PURSUE UAP archive](https://www.war.gov/ufo) ን የሚፈለግ፣ ብዙ ቋንቋ ተናጋሪ ዳግም ግንባታ) ን የყალიቡት ሶስት ውሳኔዎች ላይ ማስታወሻዎች። አስተያየቶች / ተቃውሞዎች እንኳን ደህና መጡ።

### 1. የ pipeline ሆን ተብሎ የ forward-only state machine ነው

ግዛቶች: `discovered → downloaded → ocr_done → translated → published`። አንድ ሰነድ ወደ ፊት ብቻ ይንቀሳቀሳል፣ እና ስራ ሲኖር ብቻ። የታተመ ይዘት ምንጩ በትክክል እንደተለወጠ የ delta detector ካላየ በስተቀር በጭራሽ እንደገና አይሰራም።

**ለምን:** OCR + ትርጉም ውድ ስራዎች ናቸው፣ እና መዝገብ ቤቱ በጊዜ ሂደት ያድጋል። "ለደህንነት ሲባል ሁሉንም ነገር እንደገና የሚያካሂድ" pipeline ገደብ የለሽ ወጪ አለው። ወደ ኋላ መመለስን የማይቻል ማድረግ ያልተጠበቀ ሂሳብን የማይቻል ያደርገዋል። የወጪ ጣሪያው የኦፕሬተር ንቃት ሳይሆን የመንግስት ግራፍ ንብረት ነው።

**ዋጋ:** schema migrations እና ሆን ተብሎ እንደገና ማካሄድ ሆን ተብሎ አስቸጋሪ ናቸው። ተቀባይነት ያለው ስምምነት።

### 2. OCR እና ትርጉም የሚሰሩት በአካባቢ LLM ላይ ነው፣ በክላውድ API ላይ አይደለም

OCR: open-source engine, Tesseract CLI fallback. ትርጉም + NER: Gemma via Ollama, በ Apple Silicon ላፕቶፕ ላይ።

**ለምን:** በያንዳንዱ ሰነድ ዜሮ ተጨማሪ ወጪ፤ ሊባዛ የሚችል (ቋሚ ሞዴል + ጥያቄዎች)፤ እና የመውሰጃው ደረጃ ቀድሞውኑ ከመኖሪያ ቤት IP መስራት አለበት (ምንጩ ከ Akamai Bot Manager ጀርባ ነው — `curl` 403 ያገኛል)፣ ስለዚህ ላፕቶፕ ለማንኛውም በሂደቱ ውስጥ አለ።

**ዋጋ:** የትርጉም ጥራት ከድንበር ሞዴል በታች ነው። ዋናው እንግሊዝኛ ሁል ጊዜ አንድ ጠቅታ ርቀት ላይ ላለ የማጣቀሻ ኮርፐስ፣ ያ ጥሩ ነው። ትርጉሞቹ ስልጣን እንዳላቸው አንልም።

### 3. ሁለቱ ግማሾች በትክክል አንድ በይነገጽ ይጋራሉ፦ የታተመ bundle

pipeline በቀጥታ ወደ ፕሮዳክሽን ዳታቤዝ በጭራሽ አይጽፍም። `{ SQL, asset manifest, cache-purge list }` ያወጣል። "ማተም" = ያንን bundle ወደ ፊት መተግበር (SQL ን ወደ edge SQL DB መግፋት፣ assets ን ወደ object storage ማመሳሰል፣ የተሰየሙትን cache keys ማጽዳት)።

**ለምን:** የአካባቢው ጎን እና የኤጅ ጎን በተናጥል ሊለወጡ ይችላሉ፤ bundleው ሊገመገም ይችላል፤ እና "deploy data" ሁል ጊዜ ተመሳሳይ ቅርፅ አለው። Worker ትንሽ የ TypeScript/Hono app ነው — ጥብቅ CSP ( `unsafe-inline` የለም፤ inline JSON-LD sha256-pinned ነው)፣ `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — እና ውሂቡ እንዴት እንደተሰራ ማወቅ በጭራሽ አያስፈልገውም።

**ዋጋ:** የ D1 schema ለውጥ ሁለት ፋይሎችን ይነካል (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`)። ርካሽ ዋስትና።

### በባህሪ ውስጥ የተጋገሩ የማይደራደሩ ነገሮች

- ከዩናይትድ ስቴትስ መንግስት ጋር ግንኙነት የለውም፤ ምንም ይፋዊ አርማ የለም።
- የምንጭ ስረዛዎች ተጠብቀዋል፣ በጭራሽ አይገለበጡም።
- ቪዲዮ ለ DVIDS / AARO የተሰጠ።
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` በጣቢያ-ሰፊ — በፍለጋ-ሊጠቆም የሚችል፣ ከ AI-scrape-የወጣ።

ቀጥታ ስርጭት: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
