# GitHub — Chapisho la 2 kati ya 3 · Wito wa wachangiaji / "masuala mazuri ya kuanzia"

**Tumia kama:** Majadiliano yaliyobandikwa ("Kuchangia & masuala mazuri ya kuanzia") au utangulizi wa CONTRIBUTING.md.
**Maneno muhimu:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Viungo:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kuchangia kwenye ufolens.com

[ufolens.com](https://www.ufolens.com) inageuza [hazina ya PURSUE UAP](https://www.war.gov/ufo) ya Idara ya Vita ya Marekani kuwa jukwaa linaloweza kutafutwa, la lugha nyingi na lenye [API ya umma](https://www.ufolens.com/api/v1). Ni nusu mbili — pipeline ya ndani ya Python ya kuingiza data (`pipeline/`) na programu ya pembeni ya TypeScript/Hono (`worker/`) — zinazokutana kwenye kiolesura kimoja: kifurushi kilichochapishwa cha SQL + mali.

Huhitaji stakabadhi zozote za wingu ili kuchangia. Moduli za msingi za pipeline ni za stdlib-pekee na majaribio ya Worker hufanyika kwenye hifadhi ya ndani ya kumbukumbu.

### Usanidi

```bash
# pipeline
python3 -m pytest pipeline/tests/          # inapaswa kuwa yote kijani, hakuna usakinishaji wa pip unaohitajika

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Mahali ambapo msaada unahitajika zaidi

**i18n / ujanibishaji** — `worker/src/i18n/ui-strings.json` ndicho chanzo cha maandishi ya kiolesura. Mapitio ya mzungumzaji asilia wa lugha yoyote isiyo ya Kiingereza yana thamani kubwa: gundua matokeo ya tafsiri ya mashine yasiyo ya kawaida, rekebisha masuala ya RTL/mpangilio, boresha hali za pembeni za mazungumzo ya lugha.

**Ubora wa OCR** — uchakataji bora wa awali wa nakala za zamani zilizochapwa kwa taipureta kabla ya OCR; mfumo wa tathmini unaolinganisha injini ya chanzo-wazi dhidi ya mbadala ya Tesseract kwenye kurasa za sampuli.

**Ufikivu** — kagua kurasa zilizotolewa (`worker/src/render/`) dhidi ya WCAG; CSP ni kali (hakuna `unsafe-inline`), kwa hivyo masuluhisho lazima yafanye kazi ndani ya hapo.

**Urahisi wa matumizi ya API** — `worker/src/routes/` — urasishaji, uchujaji, maelezo ya OpenAPI, wateja wa mfano.

**Uthabiti wa Pipeline** — njia zaidi za kushuka hadhi kwa urahisi, ripoti bora za maendeleo, hali za pembeni za ugunduzi wa tofauti (`pipeline/lib/delta.py`).

**Nyaraka** — `docs/20260511/` (繁體中文; `00-*` ni faharasa). Tafsiri za nyaraka za usanifu kwenda Kiingereza zinakaribishwa.

### Kanuni za msingi

- Njia zote ni za jamaa — mradi lazima uweze kuhamishika kati ya mashine. Hakuna njia kamili zilizowekwa kwa msimbo.
- Usiongeze utegemezi wa pip kwenye moduli ya *msingi* ya pipeline. Hatua za hiari zinaweza kutumia vifurushi vya hiari, na lazima zishuke hadhi kwa urahisi bila hivyo.
- Usidhoofishe mfumo wa hali ya usonge mbele pekee — hiyo ndiyo dari ya gharama.
- Usiingize nembo rasmi za serikali ya Marekani, na usiongeze chochote kinachobadilisha maeneo yaliyofichwa kwenye chanzo.
- Mabadiliko ya skema ya D1 yanagusa faili **mbili**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Majaribio na msimbo mpya. Ujumbe wa Conventional-commit.

Soma `CLAUDE.md` na `docs/20260511/00-*` kwanza, kisha fungua suala kujadili jambo lolote la kimuundo kabla ya PR.
