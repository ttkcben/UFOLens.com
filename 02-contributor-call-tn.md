# GitHub — Kwalo ya 2 mo go 3 · Pitso ya Motlatsitirisi / "dikgang tse dintle tsa go simolola"

**Dirisa jaaka:** kgang e e beilweng kwa godimo (pinned Discussion) ("Contributing & good first issues") kgotsa tshimologo ya CONTRIBUTING.md.
**Mafoko a botlhokwa:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Dikgokagano:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Go tlatsa mo ufolens.com

[ufolens.com](https://www.ufolens.com) e fetola [PURSUE UAP archive](https://www.war.gov/ufo) ya U.S. War Department go nna sethala se se patlisisegang, sa dipuo tse dintsi ka [public API](https://www.ufolens.com/api/v1). Ke dikarolo tse pedi — local Python ingest pipeline (`pipeline/`) le TypeScript/Hono edge app (`worker/`) — di kopana kwa segokaganyong se le sengwe: sephuthelwana se se phatlaladitsweng sa SQL + assets.

Ga o tlhoke ditlhotlho tsa cloud go tlatsa. Dikgwele tsa pipeline di na le stdlib-only mme ditekanyetso tsa Worker di dirisiwa kgatlhanong le in-memory storage.

### Thulaganyo

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kwa thuso e tlhokegang thata

**i18n / localization** — `worker/src/i18n/ui-strings.json` ke motswedi wa ditlhaka tsa UI. Tlhatlhobo ya sebui sa legae sa puo nngwe le nngwe e e seng Seesemane e botlhokwa thata: tshwara diphitlhelelo tsa metšhine tse di sa siamang, lokisa dikgang tsa RTL/layout, tokafatsa ditsela tsa dipuisano tsa puo.

**Boleng jwa OCR** — tokafatsa preprocessing ya diskenara tsa kgale tsa typewriter pele ga OCR; thulaganyo ya tlhathobo e e bapisa open-source engine le Tesseract fallback mo dipampiring tsa sekao.

**Phitlhelelo** — tlhatlhoba dipampiri tse di dirilweng (`worker/src/render/`) kgatlhanong le WCAG; CSP e gagametse (ga go `unsafe-inline`), ka jalo ditharabololo di tshwanetse go dira mo teng ga yone.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, dikhasimende tsa sekao.

**Pipeline robustness** — ditsela tse di tokafatsang tsa go senyega, pego ya tswelelopele e e botoka, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ke index). Dithaloso tsa ditokomane tsa thulaganyo go Seesemane di a amogelesega.

### Melao ya motheo

- Ditsela tsotlhe di a amana — tiro e tshwanetse go kgona go isiwa go metšhine e mengwe. Ga go ditsela tse di sa siamang tse di beilweng ka thata.
- Se ke wa oketsa pip dependency go pipeline *core* module. Dikgato tsa boikemelo di ka dirisa diphuthelwana tsa boikemelo, mme di tshwanetse go fokotsega sentle ntle le tsone.
- Se ke wa koafatsa thulaganyo ya gago-fela ya maemo — ke boleng jwa godimo.
- Se ke wa tsenya letshwao la semmuso la mmuso wa U.S., mme se ke wa oketsa sepe se se busetsang morago dintlha tse di fitlhilweng.
- D1 schema changes di ama difaele **tse pedi**: `pipeline/lib/manifest_schema.sql` le `db/schema.sql`.
- Ditekanyetso ka khoutu e ntšha. Conventional-commit messages.

Bala `CLAUDE.md` le `docs/20260511/00-*` pele, mme o bule kgang go buisana ka sengwe le sengwe se se amanang le popo pele ga PR.

