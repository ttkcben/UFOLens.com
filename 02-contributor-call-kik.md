# GitHub — Kibandi 2 harĩ 3 · Mwĩto wa kũgĩa na mũhothi / "wĩra mwega wa kwambĩrĩria"

**Hũthĩra ta:** Discussion ĩgwetetwo ("Kũgĩa na Mũhothi & wĩra mwega wa kwambĩrĩria") kana kĩambĩrĩria kĩa CONTRIBUTING.md.
**Ciugo cia bata:** open source, kũgĩa na mũhothi, wĩra mwega wa kwambĩrĩria, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kũgĩa na Mũhothi thĩinĩ wa ufolens.com

[ufolens.com](https://www.ufolens.com) ĩgarũraga mũthithũ wa [PURSUE UAP wa Departmenti ya Mbaara ya U.S.](https://www.war.gov/ufo) gũtuĩka kĩuga kĩa gũcaria maũndũ, kĩa ndimi nyingĩ na kĩrĩ na [API yaherie](https://www.ufolens.com/api/v1). Nĩ gĩcunjĩ kĩrĩ na icunjĩ igĩrĩ — pipeline ya Python ya kũingiza ya kũu (`pipeline/`) na app ya TypeScript/Hono ya edge (`worker/`) — ikĩranagĩra na njĩra o ĩmwe: mũkũnga-watho wa SQL + indo wathirĩirio.

Ndũrabatara ceredeni ya cloud nĩguo ũhote kũgĩa na mũhothi. Module cia mũthingi cia pipeline nĩ cia stdlib-only na magerio ma Worker marutagĩrwo wĩra na hũngĩro ya in-memory.

### Kwĩhanda

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Harĩa ũteithio ũrabatarania makĩria

**i18n / Ũgarũri na Uingirithio wa Rũrĩmĩ** — `worker/src/i18n/ui-strings.json` nĩ kĩo kĩhumo kĩa ciugo cia UI. Gũthuthuria kwa aria maragia rũrĩmĩ rũcio kwa uuma kwa rũrĩmĩ rũrĩa rũtarĩ Gĩthũngũ nĩ kwa bata mũno: kuona ciugo cia macini itarĩ njega, kũrũnga mathĩna ma RTL/mũhianĩre, kũgemia njĩra cia kwarĩrĩrio rũrĩmĩ.

**Ũhoti wa OCR** — kũrũnga mbere ya wĩra wa kũruta wĩra ndumenti cia tene cia taipu mbere ya OCR; kĩgerio gĩa kũringithania injini ya open-source na fallback ya Tesseract na peji cia kĩgerio.

**Ũhoti wa Kũhũthĩka nĩ Andũ Othe** — gũthuthuria peji iria ciarũgamirio (`worker/src/render/`) kuringana na WCAG; CSP nĩ ya hali ya igũrũ (gũtirĩ `unsafe-inline`), kwoguo njĩra cia kũniina mathĩna no mũhaka irute wĩra thĩinĩ wa mĩhaka ĩyo.

**Ũhoti wa Kũhũthĩka kwa API** — `worker/src/routes/` — pagination, kũthondeka, maelezo ma OpenAPI, mĩhiano ya kĩraenti.

**Ũgumu wa Pipeline** — njĩra nyingĩ cia kũhoyagĩria wega, kuripota kwega kwa wĩra, mathĩna ma gũthima delta (`pipeline/lib/delta.py`).

**Ndumenti** — `docs/20260511/` (繁體中文; `00-*` nĩyo index). Matafsiri ma ndumenti cia mũhianĩre nginya Gĩthũngũ nĩ maramwarĩrwo.

### Mawatho ma Mũthingi

- Njĩra ciothe nĩ cia kũhũthahũthika — mũradi no mũhaka ũhote gũkũrwo na kũrũgamio macini-inĩ itiganĩte. Gũtirĩ njĩra ndũmu cia kũrũgamio.
- Tiga gwongerera dependency ya pip harĩ module ya *mũthingi* ya pipeline. Ciĩko cia kwĩyendera no ihũthĩre pakeji cia kwĩyendera, na no mũhaka ihoyagĩrie wega rĩrĩa itirĩ kuo.
- Tiga kũnogia mũtaratara wa forward-only wa state — ĩyo nĩyo mĩhaka ya garama.
- Tiga gwongerera rũũri rwa kĩthirikari rwa U.S., na tiga gwongerera kĩndũ kĩngĩgarũra macacĩ ma kĩambĩrĩria.
- Mogarũrũku ma D1 schema mahutagia faili **igĩrĩ**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Magerio na kodi njerũ. Mũhiano wa Conventional-commit wa ndũmĩrĩri.

Thoma `CLAUDE.md` na `docs/20260511/00-*` mbere, wacie kũhingũra kĩria gĩa kwarĩrĩrio gĩkoniĩ mũhianĩre mbere ya PR.
