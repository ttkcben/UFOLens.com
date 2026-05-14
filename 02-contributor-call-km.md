# GitHub — ប្រកាសទី 2 ក្នុងចំណោម 3 · ការអំពាវនាវរកអ្នករួមចំណែក / "good first issues"

**ប្រើជា៖** ការពិភាក្សាដែលបានខ្ទាស់ ("ការរួមចំណែក & good first issues") ឬការណែនាំ CONTRIBUTING.md។
**ពាក្យគន្លឹះ៖** open source, ការរួមចំណែក, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**តំណខ្ពស់៖** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ការរួមចំណែកដល់ ufolens.com

[ufolens.com](https://www.ufolens.com) បំប្លែងបណ្ណសារ [PURSUE UAP archive](https://www.war.gov/ufo) របស់ក្រសួងសង្រ្គាមសហរដ្ឋអាមេរិក ទៅជាវេទិកាពហុភាសា និងអាចស្វែងរកបាន ជាមួយនឹង [API សាធារណៈ](https://www.ufolens.com/api/v1)។ វាមានពីរផ្នែក — pipeline ingest ក្នុងតំបន់ bằng Python (`pipeline/`) និងកម្មវិធី edge TypeScript/Hono (`worker/`) — ដែលជួបគ្នានៅចំណុចប្រទាក់តែមួយ៖ កញ្ចប់ SQL + assets ដែលបានបោះពុម្ពផ្សាយ។

អ្នកមិនត្រូវការអត្តសញ្ញាណប័ណ្ណ cloud ណាមួយដើម្បីរួមចំណែកទេ។ ម៉ូឌុលស្នូលរបស់ pipeline គឺ stdlib-only ហើយការតេស្ត Worker ដំណើរការប្រឆាំងនឹង in-memory storage។

### ការដំឡើង

```bash
# pipeline
python3 -m pytest pipeline/tests/          # គួរតែមានពណ៌បៃតងទាំងអស់, មិនត្រូវការ pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### កន្លែងដែលជំនួយមានប្រយោជន៍បំផុត

**i18n / localization** — `worker/src/i18n/ui-strings.json` គឺជាប្រភពនៃខ្សែអក្សរ UI។ ការត្រួតពិនិត្យដោយអ្នកនិយាយដើមនៃភាសាណាមួយក្រៅពីភាសាអង់គ្លេសមានតម្លៃខ្ពស់៖ ចាប់យកលទ្ធផលម៉ាស៊ីនដែលឆ្គាំឆ្គង, កែសម្រួលបញ្ហា RTL/layout, និងកែលម្អករណីពិសេសនៃការចរចាភាសា។

**គុណភាព OCR** — ការធ្វើ tiền-xử lý កាន់តែប្រសើរលើការស្កេនឯកសារចាស់ដែលបានវាយបញ្ចូលមុនពេល OCR; ប្រព័ន្ធវាយតម្លៃដែលប្រៀបធៀបម៉ាស៊ីន open-source ជាមួយ Tesseract fallback លើទំព័រគំរូ។

**Accessibility** — ធ្វើសវនកម្មលើទំព័រដែលបាន render (`worker/src/render/`) ប្រឆាំងនឹង WCAG; CSP មានភាពតឹងរ៉ឹង (គ្មាន `unsafe-inline`), ដូច្នេះដំណោះស្រាយត្រូវតែដំណើរការក្នុងក្របខ័ណ្ឌនោះ។

**API ergonomics** — `worker/src/routes/` — pagination, filtering, ការពិពណ៌នា OpenAPI, ឧទាហរណ៍ clients។

**ភាពរឹងមាំរបស់ Pipeline** — ផ្លូវ graceful-degradation កាន់តែច្រើន, ការរាយការណ៍វឌ្ឍនភាពកាន់តែប្រសើរ, ករណីពិសេសនៃការរកឃើញ delta (`pipeline/lib/delta.py`)។

**Docs** — `docs/20260511/` (繁體中文; `00-*` គឺជាលិបិក្រម)។ សូមស្វាគមន៍ការបកប្រែឯកសាររចនាទៅជាភាសាអង់គ្លេស។

### ច្បាប់មូលដ្ឋាន

- ផ្លូវទាំងអស់ជា relative — គម្រោងត្រូវតែអាចចល័តបានរវាងម៉ាស៊ីន។ គ្មានផ្លូវ absolute ដែលបាន hardcode ទេ។
- កុំបន្ថែម pip dependency ទៅម៉ូឌុល *core* របស់ pipeline។ ដំណាក់កាលស្រេចចិត្តអាចប្រើកញ្ចប់ស្រេចចិត្ត, និងត្រូវតែ degrade gracefully ដោយគ្មានពួកវា។
- កុំធ្វើឱ្យ state machine ដែលទៅមុខតែប៉ុណ្ណោះចុះខ្សោយ — នោះគឺជាការកំណត់ការចំណាយ។
- កុំណែនាំសញ្ញាសម្គាល់ផ្លូវការរបស់រដ្ឋាភិបាលសហរដ្ឋអាមេរិក, ហើយកុំបន្ថែមអ្វីដែលបញ្ច្រាសការកែសម្រួលប្រភព។
- ការផ្លាស់ប្តូរ D1 schema ប៉ះពាល់ដល់ឯកសារ **ពីរ**៖ `pipeline/lib/manifest_schema.sql` និង `db/schema.sql`។
- ការតេស្តជាមួយកូដថ្មី។ សារ Conventional-commit។

សូមអាន `CLAUDE.md` និង `docs/20260511/00-*` ជាមុនសិន, បន្ទាប់មកបើក issue ដើម្បីពិភាក្សាអំពីអ្វីដែលមានលក្ខណៈរចនាសម្ព័ន្ធមុនពេល PR។

