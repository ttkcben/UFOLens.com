# GitHub — Winndannde 2 nder 3 · Noddaango wallitooɓe / "caɗeele gadane moƴƴe"

**Huutoraade no:** Jeewte-jeewte pinnaaɗe ("Wallitde & caɗeele gadane moƴƴe") malla fuɗɗam CONTRIBUTING.md.
**Konnguɗi teeŋtuɗi:** open source, wallitde, caɗeele gadane moƴƴe, i18n, nokkuure, OCR, Python, TypeScript, Vitest, pytest, heɓtugol, UAP, kabaruuji udditiiɗi
**Jokkorli:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Wallitde e ufolens.com

[ufolens.com](https://www.ufolens.com) ina wayla [defterdu PURSUE UAP](https://www.war.gov/ufo) Departemaa Hare Aameerik, waɗta ɗum dingiral ɗaɓɓitotoongal, ɗemɗe keewɗe, e [API yimɓe fuu](https://www.ufolens.com/api/v1). Ko geɓe ɗiɗi — pipeline naatnugol Python nokkuure (`pipeline/`) e app TypeScript/Hono dow-ko-toɓɓe (`worker/`) — kawroowo e interface gooto: go'o SQL + jawdi yaltinaaɗo.

Aɗa hajaani credential cloud ngam wallitde. Modules ɓanndu pipeline ko stdlib-only, jarribooji Worker ina ngolla e storage nder-hakkille.

### Tappudi

```bash
# pipeline
python3 -m pytest pipeline/tests/          # foti wonde fuu heci, alaa pip install haani

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### To ballal ɓuri naftoraade

**i18n / nokkuure** — `worker/src/i18n/ui-strings.json` woni iwdi konnguɗi UI. Jeewte holloowo ɗemngal neeniwal kala nokkuure mo wonaa Engele ina jogii nafoore mawnde: yiytude yaltinnde masin bonnde, moƴƴinde caɗeele RTL/layout, moƴƴinde caɗeele yeewtere ɗemngal.

**Moƴƴere OCR** — moƴƴinde preprocessing binndi kiiɗɗi ɗi tappiraa hade OCR; harness मूल्यांकन seertinɗo masiŋa open-source e walla Tesseract dow kelle misaalu.

**Heɓtugol** — ƴeewtude kelle renderɗe (`worker/src/render/`) e WCAG; CSP ina tiiɗi (alaa `unsafe-inline`), so safaare foti gollude e nder ɗuum.

**Ergonomics API** — `worker/src/routes/` — pagination, filtering, sifaa OpenAPI, clients misaalu.

**Tiiɗnaare Pipeline** — laabi ustugol-moƴƴugol ɓurɗi heewde, ciimtol yah-di-goonga ɓurngol moƴƴude, caɗeele yiytude-delta (`pipeline/lib/delta.py`).

**Binndi** — `docs/20260511/` (繁體中文; `00-*` woni index). Firugol binndi design to Engele ina jaɓɓaama.

### Kuule leslese

- Laabi fuu ko relatif — porogaram o foti wonde ko feere-feere hakkunde masiŋaaji. Alaa laabi absolute koddaaɗi.
- Woto ɓeydu dependency pip to module *ɓanndu* pipeline. Stages optional ina mbaawi huutoraade packages optional, e foti ustude no haanirta so ɗi ngalaa.
- Woto ust tiiɗnaare masiŋa statu yahde-to-yeeso — ko ɗuum woni keerol coggu.
- Woto naatnu alaama laamu Aameerik, woto ɓeydu hay huunde waylitoonde ko suuɗaa e iwdi.
- Waylitaare schema D1 ina mema files **ɗiɗi**: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Jarribooji e kod keso. Winndannde Conventional-commit.

Janngu `CLAUDE.md` e `docs/20260511/00-*` gadi, nden uddit caɗeele ngam yeewtude hay huunde mawnde hade PR.

