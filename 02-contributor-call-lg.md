# GitHub — Ekiwandiko 2 kya 3 · Okuyita ab'okuyamba / "emirimu egisooka egirungi"

**Kozesa nga:** Okunyumya okusimbiddwa ("Okuyamba n'emirimu egisooka egirungi") oba ennyanjula ya CONTRIBUTING.md.
**Ebigambo ebikulu:** open source, okuyamba, emirimu egisooka egirungi, i18n, okuteeka mu lulimi olw'ekitundu, OCR, Python, TypeScript, Vitest, pytest, obusoboyozi, UAP, data enzigule
**Enkolagana:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Okuyamba ku ufolens.com

[ufolens.com](https://www.ufolens.com) efuula enkumu ya [PURSUE UAP archive](https://www.war.gov/ufo) ey'ekitongole ky'olutalo eky'e U.S. okuba olukalala olusobola okunoonyezebwa, olw'ennimi nnyingi n'[API ya lwona](https://www.ufolens.com/api/v1). Z'ebitundu bibiri — pipeline y'okuyingiza ya Python ey'ewaka (`pipeline/`) n'appu ya TypeScript/Hono ey'oku nsalo (`worker/`) — nga zisisinkana ku kintu kimu: omuganda gwa SQL + assets ogutongozeddwa.

Tewetaaga lukusa lwa kire okuyamba. Ebitundu ebikulu ebya pipeline bya stdlib-only era ebigezo bya Worker bikolera ku terekero lya mu bwongo.

### Okuteekateeka

```bash
# pipeline
python3 -m pytest pipeline/tests/          # byonna birina okuba kiragala, tewali pip install eyetaagibwa

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Wa obuyambi we bwetaagibwa ennyo

**i18n / localization** — `worker/src/i18n/ui-strings.json` y'ensibuko y'ebigambo bya UI. Okwekenneenya kw'omuntu ayogera olulimi lwe enzaalwa okw'olulimi olutali Lungereza kwa mugaso nnyo: okukwata ebivvuunuliddwa mu ngeri etategeerekeka, okutereeza ebizibu bya RTL/enkola, n'okulongoosa embeera ez'enjawulo ez'okutegeeragana kw'olulimi.

**Omutindo gwa OCR** — okuteekateeka obulungi ebiwandiiko ebyakubibwa ku kyapa edda nga OCR tennakola; enkola y'okukebera egezesa enjini ya open-source n'eya Tesseract fallback ku mpapula ez'okulabirako.

**Obusoboyozi** — kebera emiko egikoleddwa (`worker/src/render/`) okusinziira ku WCAG; CSP nkakali (tewali `unsafe-inline`), n'olwekyo ebibonerezo birina okukola munda mu yo.

**Obwangu bwa API** — `worker/src/routes/` — okupanga emiko, okusengeka, okunnyonnyola kwa OpenAPI, n'abakozesa ab'okulabirako.

**Obugumu bwa Pipeline** — amakubo amalala ag'okukola obulungi, okulaga obulungi enkulaakulana, n'embeera ez'enjawulo ez'okulaba enkyukakyuka (`pipeline/lib/delta.py`).

**Ebiwandiiko** — `docs/20260511/` (繁體中文; `00-*` ye ndagiriro). Okuvvuunula ebiwandiiko by'enteekateeka mu Lungereza kyanirizibwa.

### Amateeka ag'okugoberera

- Amakubo gonna galina okuba ga kika kimu — pulojekiti erina okusobola okutambulira ku kompyuta ez'enjawulo. Tewali makubo makakafu agateekeddwamu.
- Toweereza nkolagana ya pip ku kitundu kya pipeline *ekikulu*. Ebitundu eby'okulondawo bisobola okukozesa pakeegi ez'okulondawo, era birina okukola obulungi nga teziriiwo.
- Tonn weakened enkola y'okukola ekiddako yokka — eyo y'ekkomo ly'omuwendo.
- Toweereza birango bitongole bya gavumenti ya U.S., era toweereza kintu kyonna ekisattulula ebyagibwamu mu nsibuko.
- Enkyukakyuka mu D1 schema zikwata ku fayiro **bbiri**: `pipeline/lib/manifest_schema.sql` ne `db/schema.sql`.
- Ebigezo n'enkola empya. Obubaka bwa Conventional-commit.

Soma `CLAUDE.md` ne `docs/20260511/00-*` oluvannyuma oggulewo ekip new PR.

