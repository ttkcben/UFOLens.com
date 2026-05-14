# GitHub — Post 3 of 3 · ဗိသုကာဆိုင်ရာမှတ်စုများ (ADR-ပုံစံ ဆွေးနွေးချက်)

**အသုံးပြုရန်:** "Show and tell" / "Architecture" အောက်ရှိ ဆွေးနွေးချက် သို့မဟုတ် `docs/` ADR အစပျိုးချက်။
**Keywords:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com ကို ဘာကြောင့် ဒီလိုတည်ဆောက်ထားသလဲ

[ufolens.com](https://www.ufolens.com) ([PURSUE UAP archive](https://www.war.gov/ufo) ကို ရှာဖွေနိုင်ပြီး ဘာသာစကားမျိုးစုံဖြင့် ပြန်လည်တည်ဆောက်ထားခြင်း) ကို ပုံဖော်ခဲ့သော ဆုံးဖြတ်ချက်သုံးခုအပေါ် မှတ်စုများ။ မှတ်ချက်များ / အကြံပြုချက်များကို ကြိုဆိုပါသည်။

### ၁။ pipeline သည် ရှေ့သို့သာသွားသော state machine ဖြစ်သည် — ရည်ရွယ်ချက်ရှိရှိဖြင့်

States: `discovered → downloaded → ocr_done → translated → published`။ စာရွက်စာတမ်းတစ်ခုသည် ရှေ့သို့သာ ရွေ့လျားပြီး၊ လုပ်ဆောင်စရာရှိမှသာ ရွေ့လျားသည်။ publish လုပ်ပြီးသား အကြောင်းအရာကို delta detector မှ မူရင်းအမှန်တကယ် ပြောင်းလဲသွားသည်ကို မတွေ့မချင်း ပြန်လည်လုပ်ဆောင်ခြင်းမရှိပါ။

**ဘာကြောင့်လဲ:** OCR + translation သည် ကုန်ကျစရိတ်များသော လုပ်ငန်းစဉ်များဖြစ်ပြီး၊ archive သည် အချိန်နှင့်အမျှ ကြီးထွားလာသည်။ "ဘေးကင်းရန် အရာအားလုံးကို ပြန် run" သော pipeline သည် အဆုံးမရှိသော ကုန်ကျစရိတ်ရှိသည်။ နောက်ပြန်သွားခြင်းကို မဖြစ်နိုင်အောင် လုပ်ထားခြင်းကြောင့် ထိန်းမနိုင်သော ကုန်ကျစရိတ်ကို မဖြစ်နိုင်အောင် လုပ်ဆောင်ပေးသည်။ ကုန်ကျစရိတ်၏ အမြင့်ဆုံး batasan သည် operator ၏ စောင့်ကြပ်မှုကြောင့်မဟုတ်ဘဲ state graph ၏ ဂုဏ်သတ္တိကြောင့်ဖြစ်သည်။

**ကုန်ကျစရိတ်:** schema migrations နှင့် ရည်ရွယ်ချက်ရှိရှိ reprocessing လုပ်ခြင်းသည် တမင်တကာ အဆင်မပြေအောင် လုပ်ထားသည်။ လက်ခံနိုင်သော အပေးအယူတစ်ခုဖြစ်သည်။

### ၂။ OCR နှင့် translation ကို cloud API မဟုတ်ဘဲ local LLM တွင် run သည်

OCR: open-source engine, Tesseract CLI fallback။ Translation + NER: Gemma via Ollama, Apple Silicon laptop ပေါ်တွင်။

**ဘာကြောင့်လဲ:** စာရွက်စာတမ်းတစ်ခုချင်းစီအတွက် ကုန်ကျစရိတ် လုံးဝမရှိ၊ ပြန်လည်ထုတ်လုပ်နိုင်သည် (fixed model + prompts)၊ fetch အဆင့်သည် residential IP မှ run ရသည် (source သည် Akamai Bot Manager နောက်ကွယ်တွင်ရှိသည် — `curl` သည် 403 ရသည်)၊ ထို့ကြောင့် laptop သည် မဖြစ်မနေ ပါဝင်နေပြီးသားဖြစ်သည်။

**ကုန်ကျစရိတ်:** ဘာသာပြန်အရည်အသွေးသည် frontier model အောက် နိမ့်သည်။ မူရင်းအင်္ဂလိပ်ကို တစ်ချက်နှိပ်ရုံဖြင့် အမြဲရနိုင်သော reference corpus အတွက်မူ၊ ၎င်းသည် အဆင်ပြေပါသည်။ ဘာသာပြန်များသည် တရားဝင်ဖြစ်သည်ဟု မတောင်းဆိုပါ။

### ၃။ အပိုင်းနှစ်ပိုင်းသည် interface တစ်ခုတည်းကိုသာ share သည်- publish လုပ်ထားသော bundle

pipeline သည် production database သို့ တိုက်ရိုက်မရေးပါ။ ၎င်းသည် `{ SQL, asset manifest, cache-purge list }` ကို ထုတ်ပေးသည်။ "Publishing" = ထို bundle ကို ရှေ့သို့ apply လုပ်ခြင်း (SQL ကို edge SQL DB သို့ push လုပ်ခြင်း၊ assets များကို object storage သို့ sync လုပ်ခြင်း၊ အမည်ပေးထားသော cache keys များကို purge လုပ်ခြင်း)။

**ဘာကြောင့်လဲ:** local side နှင့် edge side တို့သည် သီးခြားစီ တိုးတက်ပြောင်းလဲနိုင်သည်၊ bundle ကို ပြန်လည်သုံးသပ်နိုင်သည်၊ "deploy data" သည် အကြိမ်တိုင်း ပုံစံတူဖြစ်သည်။ Worker သည် သေးငယ်သော TypeScript/Hono app တစ်ခုဖြစ်သည် — တင်းကျပ်သော CSP (no `unsafe-inline`; inline JSON-LD is sha256-pinned), `Accept-Language` + country→language negotiation, ရက် ၃၀ KV page cache, နေ့စဉ် housekeeping cron — ၎င်းသည် data ကို မည်သို့ဖန်တီးခဲ့သည်ကို သိရန်မလိုအပ်ပါ။

**ကုန်ကျစရိတ်:** D1 schema ပြောင်းလဲမှုတစ်ခုသည် ဖိုင်နှစ်ခု (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`) ကို ထိခိုက်သည်။ အလွန်တန်သော ကြိုတင်ကာကွယ်မှုတစ်ခုဖြစ်သည်။

### အပြုအမူထဲတွင် ထည့်သွင်းထားသော မဖြစ်မနေလိုက်နာရမည့်အချက်များ

- အမေရိကန်အစိုးရနှင့် ဆက်နွယ်ခြင်းမရှိ၊ တရားဝင်အမှတ်တံဆိပ်များ မသုံးပါ။
- မူရင်းပြင်ဆင်ထားသည့် အချက်အလက်များကို ထိန်းသိမ်းထားပြီး၊ မည်သည့်အခါမျှ ပြန်လည်မဖော်ထုတ်ပါ။
- ဗီဒီယိုကို DVIDS / AARO သို့ credit ပေးသည်။
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-wide — search-indexable, AI-scrape-opted-out။

တိုက်ရိုက်ကြည့်ရှုရန်: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

