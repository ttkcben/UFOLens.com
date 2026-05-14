# GitHub — Post 2 jeh 3 · Gerrym co-obbreyder / "skeealyn toshee mie"

**Ymmyd:** myr Resooneyaght festit ("Curraneyn & skeealyn toshee mie") ny myr roie-raa CONTRIBUTING.md.
**Focklyn-ogher:** foshlit-foshlit, curraneyn, skeealyn toshee mie, i18n, ynnydaghey, OCR, Python, TypeScript, Vitest, pytest, so-roshtynaght, UAP, data foshlit
**Hyperchianglaghyn:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Curraneyn da ufolens.com

[ufolens.com](https://www.ufolens.com) t'eh jannoo tasht-fysseree [PURSUE UAP](https://www.war.gov/ufo) jeh Rheynn Caggee ny SU myr ardane ronsaghey-ys, yl-hengagh lesh [API theayagh](https://www.ufolens.com/api/v1). T'eh daa lieh — pib-linney Python ynnydagh (`pipeline/`) as app oir TypeScript/Hono (`worker/`) — çheet ry-cheilley ec un eddyr-oaie: bundeil SQL + cooid soilshit magh.

Cha nel feme ayd er teishtynyssyn ooilley son curraneyn ayns shen. Ta modjulyn cree y phib-linney stdlib-ynrican as ta prowallaghyn y Worker roie noi stoyral 'sy chooinaght.

### Reaghey

```bash
# pipeline
python3 -m pytest pipeline/tests/          # lhisagh ooilley ve geayney, gyn feme er pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Raad ta cooney smoo ymmydoil

**i18n / ynnydaghey** — `worker/src/i18n/ui-strings.json` t'eh bun ny strengyn UI. Ta aa-vriwnys liorish loayreyder dooghyssagh jeh çhengey erbee nagh vel Baarle feer veasagh: gow rick er eiyrtyssyn claare neuchooie, lhiasaghey skeealyn RTL/cummagh, as feer-chiarryn co-chruinnaghey çhengey.

**Keyllid OCR** — aa-obbraghey ny share jeh shenn scannaghyn clou-screeuit roish OCR; greie meesurey cosoylaghey yn jeshaght foshlit-foshlit noi'n ergooyl Tesseract er duillagyn sampleyragh.

**So-roshtynaght** — meesurey ny duillagyn taishbynit (`worker/src/render/`) noi WCAG; ta'n CSP geyre (gyn `unsafe-inline`), myr shen lhisagh er reaghyn obbraghey çheusthie jeh shen.

**Ergonoomaght API** — `worker/src/routes/` — duillaghey, sheeleghey, cur sheese OpenAPI, cleinyn sampleyragh.

**Niart y phib-linney** — ny smoo raaidyn leodaghey-graysoil, tuarastyl çheet-er-oaie ny share, feer-chiarryn lorgey-delta (`pipeline/lib/delta.py`).

**Docamadyn** — `docs/20260511/` (繁體中文; `00-*` t'eh yn ayrdex). Ta çhyndaaghyn jeh ny docamadyn cummey dys Baarle failtit.

### Reill-ynnyd

- Ooilley ny raaidyn co-cheintyssagh — lhisagh y shaleeys ve so-ymmyrkey harrish jeshaghtyn. Gyn raaidyn follit cruinn vunnit.
- Ny cur croghaneys pip rish modjul *cree* pib-linney. Foddee keimyn reihyssagh ymmyd jeh paggaghyn reihyssagh, as lhisagh orroo leodaghey dy graysoil nyn 'eeamse.
- Ny jean annoonaghey yn jeshaght stayd er oaie-ynrican — shen mullagh y chostys.
- Ny cur stiagh cowraghyn oikoil reiltys ny SU, as ny cur stiagh veg ta cur er ash ardoilghyn bunneydagh.
- Ta caghlaaghyn skeeamey D1 bwoalley **daa** 'ile: `pipeline/lib/manifest_schema.sql` as `db/schema.sql`.
- Prowallaghyn lesh coad noa. Çhaghteraghtyn conventional-commit.

Lhaih `CLAUDE.md` as `docs/20260511/00-*` hoshiaght, eisht foshil skeeal son resooney er veg strughtooragh roish y PR.

