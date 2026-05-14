# GitHub — Kannad 2 eus 3 · Galv d'ar c'henderc'herien / "kudennoù kentañ mat"

**Implij evel:** ur gaozenn benveket ("Kemer perzh & kudennoù kentañ mat") pe ur rakger da CONTRIBUTING.md.
**Gerioù-alc'hwez:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hiperliammoù:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kemer perzh e ufolens.com

[ufolens.com](https://www.ufolens.com) a dro dielloù [PURSUE UAP an Departamant Brezel Stadunanat](https://www.war.gov/ufo) en ur savenn glaskadus ha liesyezhek gant un [API foran](https://www.ufolens.com/api/v1). Daou hanterenn eo — ur pipeline degas Python lec'hel (`pipeline/`) hag un app edge TypeScript/Hono (`worker/`) — o kejañ en un etrefas hepken : ur pakad SQL + danvezioù embannet.

N'eus ket ezhomm kredantieloù cloud ebet evit kemer perzh. Moduloù kalon ar pipeline a zo stdlib-hepken hag an amprouennoù Worker a ya en-dro a-enep ur stokadenn e-koun.

### Staliañ

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### E-lec'h ma'z eo an sikour ar muiañ talvoudus

**i18n / lec'heladur** — `worker/src/i18n/ui-strings.json` eo tarzh neudennadoù an UI. Ur gwiriadenn gant ur yezher genidik eus forzh peseurt yezh nann-saoznek a zo a dalvoudegezh uhel : tapout ezvonedoù mekanikel direizh, reizhañ kudennoù RTL/pajennaozañ, gwellaat an edge cases e marc'hataerezh ar yezh.

**Perzh an OCR** — gwelloc'h rak-prosesiñ skannoù kozh bizskrivet a-raok an OCR ; ur seurt-evaluiñ o keñveriañ ar c'heflusker open-source gant an distro Tesseract war pajennoù skouer.

**Hegerzhded** — odit ar pajennoù renderet (`worker/src/render/`) a-enep WCAG ; strizh eo ar CSP (hep `unsafe-inline`), setu e tle an diskoulmoù labourat e-barzh se.

**Ergonomiezh an API** — `worker/src/routes/` — pajennadur, silañ, deskrivadur OpenAPI, klianted skouer.

**Kadern ar Pipeline** — muioc'h a hentoù diskenn dereat, gwelloc'h danevelliñ ar araokadennoù, edge cases an detektadur delta (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` eo ar meneger). Degemeret mat eo an troidigezhioù eus an teulioù tresañ e saozneg.

### Reolennoù diazez

- An holl hentoù relativel — ar raktres a rank bezañ hezoug a-dreuz ar mekanikoù. Hent absolutel kodet-kalet ebet.
- Na ouzhpennit ket un dépendandelezh pip d'ur modul *kalon* eus ar pipeline. An tappennoù diret a c'hall implijout pakoù diret, hag a rank diskenn en un doare dereat hepte.
- Na wanait ket ar mekanik stad war-raok-hepken — se eo lein ar c'houst.
- Na zegasit ket merkoù ofisiel eus gouarnamant ar Stadoù-Unanet, ha na ouzhpennit netra a zizreol adaozadennoù an tarzh.
- Ar c'hemmoù e skema D1 a stok ouzh **daou** restr : `pipeline/lib/manifest_schema.sql` ha `db/schema.sql`.
- Amprouennoù gant kod nevez. Kemennadennoù conventional-commit.

Lennit `CLAUDE.md` ha `docs/20260511/00-*` da gentañ, ha digorit un issue evit eskemm diwar-benn kement tra strukturel a-raok ar PR.
