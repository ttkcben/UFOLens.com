# GitHub — Post 2 of 3 · Pitšo ya batšeakarolo / "dintla tše botse tša mathomo"

**Šomiša bjalo ka:** Discussion yeo e khomareditšwego ("Go tsenya letsogo & dintla tše botse tša mathomo") goba selelekela sa CONTRIBUTING.md.
**Mantšu a bohlokwa:** mothopo o bulegilego, go tsenya letsogo, taba e botse ya mathomo, i18n, go dira gore e be ya selegae, OCR, Python, TypeScript, Vitest, pytest, phitlhelelago, UAP, data yeo e bulegilego
**Dikgokagano tša inthanete:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Go tsenya letsogo go ufolens.com

[ufolens.com](https://www.ufolens.com) e fetola polokelo ya [PURSUE UAP archive](https://www.war.gov/ufo) ya Lefapha la Ntwa la U.S. go ba sethalwa seo se kgonago go nyakišišwa, sa maleme a mantši seo se nago le [API ya setšhaba](https://www.ufolens.com/api/v1). Ke diripa tše pedi — pipeline ya selegae ya Python (`pipeline/`) le app ya TypeScript/Hono ya marangrang (`worker/`) — tšeo di kopanago go sebopego se tee: sephuthelwana sa SQL + didirišwa seo se phatlaladitšwego.

Ga o hloke ditlankana tša leru go tsenya letsogo. Di-module tša mokgo wa pipeline ke tša stdlib fela gomme diteko tša Worker di sepela kgahlanong le polokelo ya ka gare ga kgopolo.

### Peakanyo

```bash
# pipeline
python3 -m pytest pipeline/tests/          # e swanetše go ba tala ka moka, ga go na pip install yeo e hlokegago

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Moo thušo e lego bohlokwa kudu

**i18n / go dira gore e be ya selegae** — `worker/src/i18n/ui-strings.json` ke mothopo wa dithapo tša UI. Tlhahlobo ya seboledi sa setlogo sa lefelo le ge e le lefe leo e sego la Seisemane e na le bohlokwa bjo bogolo: swara tšweletšo ya motšhene yeo e sa swanelago, lokiša dintla tša RTL/peakanyo, kaonefatša maemo a therisano ya polelo.

**Boleng bja OCR** — go šoma gabotse ga diswantšho tša kgale tšeo di ngwadilwego ka motšhene pele ga OCR; lenaneo la tlhahlobo leo le bapišago enjene ya mothopo o bulegilego le Tesseract fallback go matlakala a mohlala.

**Phitlhelelago** — lekola matlakala ao a filwego (`worker/src/render/`) kgahlanong le WCAG; CSP e tiile (ga go na `unsafe-inline`), ka fao ditharollo di swanetše go šoma ka gare ga yona.

**Ergonomics ya API** — `worker/src/routes/` — go ngwala matlakala, go sefa, tlhaloso ya OpenAPI, bareki ba mohlala.

**Go tiya ga pipeline** — ditsela tše di kaone tša go theoga gabotse, pego ye kaone ya tšwelopele, maemo a go lemoga phapano (`pipeline/lib/delta.py`).

**Ditaelo** — `docs/20260511/` (繁體中文; `00-*` ke lenaneo). Diphetolelo tša ditokomane tša peakanyo go Seisemane di amogetšwe.

### Melao ya motheo

- Ditsela ka moka di a amana — porojeke e swanetše go kgona go rwalwa go phatša metšhene. Ga go na ditsela tša kgonthe tšeo di ngwadilwego ka go tiya.
- O se ke wa oketša boitšhepo bja pip go module ya *mokgo* wa pipeline. Dikgato tša boikgethelo di ka šomiša diphuthelwana tša boikgethelo, gomme di swanetše go theoga gabotse ntle le tšona.
- O se ke wa fokodiša motšhene wa boemo bja go ya pele fela — ke tekanyo ya godimo ya ditshenyagalelo.
- O se ke wa oketša maswao a semmušo a mmušo wa U.S., gomme o se ke wa oketša selo le ge e le sefe seo se bušetšago morago diphetošo tša mathomo.
- Diphetogo tša schema tša D1 di ama difaele tše **pedi**: `pipeline/lib/manifest_schema.sql` le `db/schema.sql`.
- Diteko ka khoutu ye mpsha. Melaetša ya boitlamo bja setšo.

Bala `CLAUDE.md` le `docs/20260511/00-*` pele, ke moka o bule taba go ahlaahla selo le ge e le sefe sa sebopego pele ga PR.

