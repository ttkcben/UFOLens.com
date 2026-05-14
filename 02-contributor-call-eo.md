# GitHub — Afiŝo 2 el 3 · Alvoko al kontribuantoj / "bonaj unuaj taskoj"

**Uzu kiel:** alpinglita diskuto ("Kontribuado & bonaj unuaj taskoj") aŭ enkonduko por CONTRIBUTING.md.
**Ŝlosilvortoj:** malferma fonto, kontribuado, bona unua tasko, i18n, lokalizado, OCR, Python, TypeScript, Vitest, pytest, alirebleco, UAP, malfermaj datumoj
**Hiperligoj:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kontribui al ufolens.com

[ufolens.com](https://www.ufolens.com) transformas la arkivon [PURSUE pri UAP](https://www.war.gov/ufo) de la Usona Departemento de Milito en serĉeblan, plurlingvan platformon kun [publika API](https://www.ufolens.com/api/v1). Ĝi konsistas el du duonoj — loka Python-ingesta dukto (`pipeline/`) kaj TypeScript/Hono-randa aplikaĵo (`worker/`) — kiuj renkontiĝas ĉe unu interfaco: publikigita pakaĵo de SQL + aktivaĵoj.

Vi ne bezonas nubajn akreditaĵojn por kontribui. La kernaj moduloj de la dukto uzas nur la norman bibliotekon kaj la testoj de la `Worker` ruliĝas kontraŭ enmemora stokado.

### Agordo

```bash
# dukto
python3 -m pytest pipeline/tests/          # ĉiuj devus esti verdaj, neniu pip-instalo bezonata

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kie helpo estas plej utila

**i18n / lokalizado** — `worker/src/i18n/ui-strings.json` estas la fonto de UI-tekstoj. Revizio fare de denaska parolanto de iu ajn ne-angla lokaĵo estas tre valora: kapti mallertajn maŝinajn rezultojn, ripari RTL/aranĝajn problemojn, plibonigi randajn kazojn de lingva negocado.

**OCR-kvalito** — pli bona antaŭ-prilaborado de malnovaj tajpitaj skanaĵoj antaŭ OCR; taksa sistemo komparanta la malfermfontan motoron kontraŭ la Tesseract-rezervon sur specimenaj paĝoj.

**Alirebleco** — reviziu la redonitajn paĝojn (`worker/src/render/`) kontraŭ WCAG; la CSP estas strikta (neniu `unsafe-inline`), do solvoj devas funkcii ene de tio.

**API-ergonomio** — `worker/src/routes/` — paĝigo, filtrado, priskribo per OpenAPI, ekzemplaj klientoj.

**Dukta fortikeco** — pli da vojoj por gracia degenero, pli bona raportado de progreso, randaj kazoj por diferenc-detektado (`pipeline/lib/delta.py`).

**Dokumentoj** — `docs/20260511/` (繁體中文; `00-*` estas la indekso). Tradukoj de la dezajnaj dokumentoj al la angla estas bonvenaj.

### Bazaj reguloj

- Ĉiuj vojoj estu relativaj — la projekto devas esti portebla inter maŝinoj. Neniuj fiksitaj absolutaj vojoj.
- Ne aldonu `pip` dependon al *kerna* modulo de la dukto. Nedevigaj stadioj povas uzi nedevigajn pakaĵojn, kaj devas gracie degeneri sen ili.
- Ne malfortigu la nur-antaŭen statmaŝinon — tio estas la kosta plafono.
- Ne enkonduku oficialajn insignojn de la usona registaro, kaj ne aldonu ion, kio malfaras fontajn redaktojn.
- `D1` skemaj ŝanĝoj tuŝas **du** dosierojn: `pipeline/lib/manifest_schema.sql` kaj `db/schema.sql`.
- Testoj kun nova kodo. Mesaĝoj de `Conventional-commit`.

Legu `CLAUDE.md` kaj `docs/20260511/00-*` unue, poste malfermu problemon por diskuti ion strukturan antaŭ la PR.

