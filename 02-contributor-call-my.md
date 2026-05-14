# GitHub — Post 2 of 3 · ပူးပေါင်းပါဝင်ရန်ခေါ်ဆိုခြင်း / "စတင်ရန်လွယ်ကူသော issues"

**အသုံးပြုရန်:** ပင်ထိုးထားသော ဆွေးနွေးချက် ("Contributing & good first issues") သို့မဟုတ် CONTRIBUTING.md ၏ အဖွင့်စာ။
**Keywords:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com သို့ ပူးပေါင်းပါဝင်ခြင်း

[ufolens.com](https://www.ufolens.com) သည် အမေရိကန် စစ်ဘက်ဌာန၏ [PURSUE UAP archive](https://www.war.gov/ufo) ကို ရှာဖွေနိုင်သော၊ ဘာသာစကားမျိုးစုံသုံး ပလက်ဖောင်းတစ်ခုအဖြစ် [public API](https://www.ufolens.com/api/v1) နှင့်အတူ ပြောင်းလဲပေးထားပါသည်။ ၎င်းတွင် အပိုင်းနှစ်ပိုင်းပါဝင်သည် — local Python ingest pipeline (`pipeline/`) နှင့် TypeScript/Hono edge app (`worker/`) တို့ဖြစ်ပြီး၊ publish လုပ်ထားသော SQL + assets bundle ဆိုသည့် interface တစ်ခုတည်းတွင် ဆုံတွေ့ကြသည်။

ပူးပေါင်းပါဝင်ရန် cloud credentials များ မလိုအပ်ပါ။ pipeline ၏ core modules များသည် stdlib-only ဖြစ်ပြီး Worker test များကို in-memory storage တွင် run နိုင်ပါသည်။

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # အားလုံးစိမ်းနေသင့်သည်၊ pip install မလိုအပ်ပါ

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### အကူအညီအလိုအပ်ဆုံးနေရာများ

**i18n / localization** — `worker/src/i18n/ui-strings.json` သည် UI စာသားများ၏ မူရင်းနေရာဖြစ်သည်။ မည်သည့်အင်္ဂလိပ်မဟုတ်သော locale ကိုမဆို မိခင်ဘာသာစကားအဖြစ် ပြောဆိုသူမှ ပြန်လည်သုံးသပ်ခြင်းသည် အလွန်တန်ဖိုးရှိပါသည်- မပြေပြစ်သောစက်ဘာသာပြန်များကို ဖမ်းယူခြင်း၊ RTL/layout ပြဿနာများကို ပြင်ဆင်ခြင်း၊ language-negotiation edge cases များကို ပိုမိုကောင်းမွန်အောင်ပြုလုပ်ခြင်း။

**OCR အရည်အသွေး** — OCR မလုပ်မီ စာစီစာရိုက်ဟောင်း scan များကို ပိုမိုကောင်းမွန်စွာ pre-processing လုပ်ခြင်း၊ open-source engine နှင့် Tesseract fallback ကို နမူနာစာမျက်နှာများတွင် နှိုင်းယှဉ်စစ်ဆေးသည့် evaluation harness။

**Accessibility** — render လုပ်ထားသော စာမျက်နှာများ (`worker/src/render/`) ကို WCAG နှင့် တိုက်ဆိုင်စစ်ဆေးခြင်း၊ CSP သည် တင်းကျပ်သောကြောင့် (no `unsafe-inline`)၊ အဖြေများသည် ထိုအတွင်း၌သာ အလုပ်လုပ်ရပါမည်။

**API ergonomics** — `worker/srcsrc/routes/` — pagination, filtering, OpenAPI description, example clients။

**Pipeline ခိုင်မာမှု** — ပိုမိုကောင်းမွန်သော graceful-degradation paths များ၊ ပိုမိုကောင်းမွန်သော progress reporting၊ delta-detection edge cases (`pipeline/lib/delta.py`)။

**Docs** — `docs/20260511/` (繁體中文; `00-*` သည် index ဖြစ်သည်)။ design docs များကို အင်္ဂလိပ်ဘာသာသို့ ပြန်ဆိုခြင်းကို ကြိုဆိုပါသည်။

### အခြေခံစည်းမျဉ်းများ

- path များအားလုံး relative ဖြစ်ရမည် — ပရောဂျက်သည် စက်များအကြား ရွှေ့ပြောင်းနိုင်ရမည်။ hardcoded absolute paths များမရှိရ။
- pipeline *core* module သို့ pip dependency မထည့်ပါနှင့်။ Optional stages များသည် optional packages များကို သုံးနိုင်ပြီး၊ ၎င်းတို့မရှိဘဲ ကောင်းမွန်စွာ အလုပ်လုပ်ရမည်။
- ရှေ့သို့သာသွားသော state machine ကို အားမပျော့ပါစေနှင့် — ၎င်းသည် ကုန်ကျစရိတ်ကို ထိန်းချုပ်ပေးသည်။
- တရားဝင် အမေရိကန်အစိုးရ အမှတ်တံဆိပ်များကို မထည့်ပါနှင့်၊ မူရင်းပြင်ဆင်ထားသည့် အချက်အလက်များကို ပြန်လည်ဖော်ထုတ်သည့် မည်သည့်အရာကိုမျှ မထည့်ပါနှင့်။
- D1 schema ပြောင်းလဲမှုများသည် `pipeline/lib/manifest_schema.sql` နှင့် `db/schema.sql` ဖိုင် **နှစ်ခု** ကို ထိခိုက်သည်။
- code အသစ်တိုင်းတွင် test များပါရမည်။ Conventional-commit messages များသုံးရမည်။

PR မလုပ်မီ structural ပြောင်းလဲမှုများအတွက် issue တစ်ခုဖွင့်ပြီး ဆွေးနွေးရန် `CLAUDE.md` နှင့် `docs/20260511/00-*` ကို ဦးစွာဖတ်ပါ။

