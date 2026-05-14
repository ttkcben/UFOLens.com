# GitHub — Post 2 o 3 · Galwad i gyfranwyr / "materion cyntaf da"

**Defnyddiwch fel:** Trafodaeth wedi'i phinio ("Cyfrannu a materion cyntaf da") neu gyflwyniad CONTRIBUTING.md.
**Allweddeiriau:** ffynhonnell agored, cyfrannu, mater cyntaf da, i18n, lleoleiddio, OCR, Python, TypeScript, Vitest, pytest, hygyrchedd, UAP, data agored
**Hypergysylltiadau:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cyfrannu i ufolens.com

Mae [ufolens.com](https://www.ufolens.com) yn troi [archif PURSUE UAP](https://www.war.gov/ufo) Adran Ryfel yr U.D. yn blatfform chwilioadwy, amlieithog gydag [API cyhoeddus](https://www.ufolens.com/api/v1). Mae'n ddwy hanner — piblinell amlyncu Python leol (`pipeline/`) ac ap ymylol TypeScript/Hono (`worker/`) — yn cwrdd mewn un rhyngwyneb: bwndel SQL + asedau wedi'u cyhoeddi.

Nid oes angen unrhyw gymwysterau cwmwl arnoch i gyfrannu. Mae modiwlau craidd y biblinell yn stdlib-yn-unig ac mae profion y Worker yn rhedeg yn erbyn storfa yn y cof.

### Gosod

```bash
# piblinell
python3 -m pytest pipeline/tests/          # dylai fod i gyd yn wyrdd, dim angen gosod pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ble mae help fwyaf defnyddiol

**i18n / lleoleiddio** — `worker/src/i18n/ui-strings.json` yw ffynhonnell llinynnau'r UI. Mae adolygiad siaradwr brodorol o unrhyw locale heblaw Saesneg o werth uchel: dal allbwn peirianyddol lletchwith, trwsio problemau RTL/cynllun, gwella achosion ymylol negodi iaith.

**Ansawdd OCR** — gwell rhag-brosesu o sganiau teipysgrif hen cyn OCR; harnais gwerthuso yn cymharu'r peiriant ffynhonnell agored yn erbyn y Tesseract wrth gefn ar dudalennau sampl.

**Hygyrchedd** — archwiliwch y tudalennau a rednir (`worker/src/render/`) yn erbyn WCAG; mae'r CSP yn llym (dim `unsafe-inline`), felly rhaid i atebion weithio o fewn hynny.

**Ergonomeg API** — `worker/src/routes/` — tudalennu, hidlo, disgrifiad OpenAPI, cleientiaid enghreifftiol.

**Cadernid y biblinell** — mwy o lwybrau israddio osgeiddig, gwell adrodd ar gynnydd, achosion ymylol canfod-delta (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` yw'r mynegai). Croesewir cyfieithiadau o'r dogfennau dylunio i'r Saesneg.

### Rheolau sylfaenol

- Pob llwybr yn gymharol — rhaid i'r prosiect fod yn gludadwy ar draws peiriannau. Dim llwybrau absoliwt wedi'u codio'n galed.
- Peidiwch ag ychwanegu dibyniaeth pip i fodiwl *craidd* piblinell. Gall camau dewisol ddefnyddio pecynnau dewisol, a rhaid iddynt israddio'n osgeiddig hebddynt.
- Peidiwch â gwanhau'r peiriant cyflwr ymlaen-yn-unig — dyna'r nenfwd cost.
- Peidiwch â chyflwyno arwyddluniau swyddogol llywodraeth yr U.D., a pheidiwch ag ychwanegu unrhyw beth sy'n gwrthdroi golygiadau'r ffynhonnell.
- Mae newidiadau i sgema D1 yn cyffwrdd â **dwy** ffeil: `pipeline/lib/manifest_schema.sql` a `db/schema.sql`.
- Profion gyda chod newydd. Negeseuon Conventional Commits.

Darllenwch `CLAUDE.md` a `docs/20260511/00-*` yn gyntaf, yna agorwch issue i drafod unrhyw beth strwythurol cyn y PR.
