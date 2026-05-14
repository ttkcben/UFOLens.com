# GitHub — Ìkéde 2 nínú 3 · Ìpè sí àwọn olùkópa / "àwọn iṣẹ́ àkọ́kọ́ tó dára"

**Lò ó bíi:** Discussion tí a lẹ̀ mọ́ ("Ṣíṣe alabapin & àwọn iṣẹ́ àkọ́kọ́ tó dára") tàbí ìbẹ̀rẹ̀ CONTRIBUTING.md.
**Àwọn ọ̀rọ̀ pàtàkì:** orísun ṣíṣí, ṣíṣe alabapin, iṣẹ́ àkọ́kọ́ tó dára, i18n, ìsọd местный, OCR, Python, TypeScript, Vitest, pytest, ìrọ̀rùn àìmọ́, UAP, data ṣíṣí
**Àwọn ìjápọ̀:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ṣíṣe Alabapin sí ufolens.com

[ufolens.com](https://www.ufolens.com) sọ [ibi-ipamọ́ PURSUE UAP](https://www.war.gov/ufo) ti Ẹ̀ka Ètò Ogun Amẹ́ríkà di pẹpẹ onírúurú èdè, tí a lè ṣàwárí, pẹ̀lú [API fún gbogbo ènìyàn](https://www.ufolens.com/api/v1). Ó jẹ́ apá méjì — pipeline ìgbàwọlé Python agbègbè kan (`pipeline/`) àti ohun èlò etí bèbè TypeScript/Hono (`worker/`) — tí wọ́n pàdé ní ojú kan: àpò SQL + àwọn ohun-ìní tí a tẹ̀ jáde.

O kò nílò àwọn ìwé-ìgbàwọlé cloud kankan láti kópa. Àwọn apá àárín pipeline jẹ́ stdlib-nìkan àti pé àwọn àyẹ̀wò Worker ń ṣiṣẹ́ lòdì sí ibi-ipamọ́ inú ẹ̀rọ.

### Ìṣètò

```bash
# pipeline
python3 -m pytest pipeline/tests/          # gbogbo rẹ̀ gbọ́dọ̀ jẹ́ aláwọ̀-ewé, kò nílò `pip install`

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ibi tí ìrànlọ́wọ́ ti wúlò jùlọ

**i18n / ìsọd agbègbè** — `worker/src/i18n/ui-strings.json` ni orísun àwọn ọ̀rọ̀ UI. Àgbéyẹ̀wò láti ọ̀dọ̀ agbọ̀rọ̀-èdè abínibí fún èdèkédè tí kì í ṣe Gẹ̀ẹ́sì ṣe pàtàkì: mú àwọn ìtumọ̀ ẹ̀rọ tí kò bójú mu, ṣatúnṣe àwọn ìṣòro RTL/ìtò, àti mú àwọn ipò ìdùnàdúrà èdè dáradára.

**Dídára OCR** — ìtọ́jú àkọ́kọ́ tó dára jù fún àwọn àwòrán ìwé àtijọ́ kí a tó ṣe OCR; ohun èlò ìṣàyẹ̀wò tí ń fi ẹ̀rọ orísun ṣíṣí wé àfikún Tesseract lórí àwọn ojú-ewé àpẹẹrẹ.

**Ìrọ̀rùn àìmọ́** — ṣàyẹ̀wò àwọn ojú-ewé tí a ṣe (`worker/src/render/`) lòdì sí WCAG; CSP náà le (kò sí `unsafe-inline`), nítorí náà àwọn ojútùú gbọ́dọ̀ ṣiṣẹ́ láàrin ìyẹn.

**Ìrọ̀rùn API** — `worker/src/routes/` — ìtòjọ-ojú-ewé, síṣẹ́, àpèjúwe OpenAPI, àwọn oníbàárà àpẹẹrẹ.

**Ìdúróṣinṣin Pipeline** — àwọn ọ̀nà ìdínkù-dáradára púpọ̀ síi, ìjábọ̀ ìlọsíwájú tó dára jù, àwọn ipò ìṣòro nínú ìwárí-ìyàtọ̀ (`pipeline/lib/delta.py`).

**Àwọn ìwé** — `docs/20260511/` (繁體中文; `00-*` ni àtòjọ). A kàábà sí ìtúmọ̀ àwọn ìwé ètò sí Gẹ̀ẹ́sì.

### Àwọn òfin ìlànà

- Gbogbo ọ̀nà ní ìbámu pẹ̀lú ipò — iṣẹ́-àkànṣe náà gbọ́dọ̀ jẹ́ èyí tí a lè gbé kọjá àwọn ẹ̀rọ. Kò sí àwọn ọ̀nà pátápátá tí a kọ sílẹ̀.
- Má ṣe fi ìgbẹ́kẹ̀lé `pip` kún apá *àárín* pipeline kan. Àwọn ìpele àṣàyàn lè lo àwọn àpò àṣàyàn, àti pé wọ́n gbọ́dọ̀ dínkù dáradára láìsí wọn.
- Má ṣe sọ ẹ̀rọ ìṣesí ìlọsíwájú-nìkan di aláìlágbára — ìyẹn ni òpin ìnáwó.
- Má ṣe fi àmì ìjọba Amẹ́ríkà osise kún un, àti pé má ṣe fi ohunkóhun tí ó ń yí àwọn àtúnṣe orísun padà kún un.
- Àwọn àyípadà ètò D1 kan àwọn faili **méjì**: `pipeline/lib/manifest_schema.sql` àti `db/schema.sql`.
- Àwọn àyẹ̀wò pẹ̀lú koodu tuntun. Àwọn ìfiranṣẹ́ àṣà-ìdáwọ́lé.

Ka `CLAUDE.md` àti `docs/20260511/00-*` ni àkọ́kọ́, lẹ́yìn náà ṣí ìṣòro kan láti jíròrò ohunkóhun tó jẹ mọ́ ìṣètò kí o tó ṣe PR.
