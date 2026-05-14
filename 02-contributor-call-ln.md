# GitHub — Post 2 of 3 · Libiangi mpo na kosunga / "ba issues ya malamu ya liboso"

**Salelá lokola:** Discussion oyo epikami ("Kosunga & ba issues ya malamu ya liboso") to ebandeli ya CONTRIBUTING.md.
**Maloba ya ntina:** open source, kosunga, issue ya malamu ya liboso, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibilité, UAP, open data
**Ba hyperliens:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kosunga na ufolens.com

[ufolens.com](https://www.ufolens.com) ebongoli [archive ya PURSUE UAP](https://www.war.gov/ufo) ya Departema ya Etumba ya Amerika na ebombelo ya bolukiluki, ya minoko mingi na [API ya bato nyonso](https://www.ufolens.com/api/v1). Ezali biteni mibale — pipeline ya Python ya esika moko (`pipeline/`) mpe application ya pembeni ya TypeScript/Hono (`worker/`) — ekutanaka na interface moko: liboke ya SQL + biloko oyo ebimisami.

Ozali na mposa ya ba credentials ya cloud te mpo na kosunga. Ba modules ya motema ya pipeline ezali stdlib-only mpe ba tests ya Worker esalaka na kati ya mémoire.

### Botongi

```bash
# pipeline
python3 -m pytest pipeline/tests/          # esengeli kozala malamu nyonso, mposa ya pip install te

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Esika lisungi ezali na ntina mingi

**i18n / localization** — `worker/src/i18n/ui-strings.json` ezali source ya maloba ya UI. Botali ya moto oyo alobaka monoko moko na moko ya mboka ezali na ntina mingi: kamata maloba ya machine oyo ezali mabe, bongisa makambo ya RTL/botongi, bongisa makambo ya ndenge ya kotala monoko.

**Bolembu ya OCR** — kobongisa malamu bilili ya kala ya mikanda ya taper avant OCR; esika ya komeka mpo na kokanisa moteur ya open-source na fallback ya Tesseract na bandakisa ya nkasa.

**Accessibilité** — talá nkasa oyo ebimisami (`worker/src/render/`) na kotalela WCAG; CSP ezali ya makasi (kozanga `unsafe-inline`), yango wana ba solutions esengeli kosala na kati na yango.

**Ergonomie ya API** — `worker/src/routes/` — pagination, filtrage, description ya OpenAPI, bandakisa ya ba clients.

**Bonguvu ya pipeline** — nzela mingi ya dégradation malamu, rapport ya progrès ya malamu, makambo ya ndelo ya détection ya delta (`pipeline/lib/delta.py`).

**Mikanda** — `docs/20260511/` (繁體中文; `00-*` ezali index). Libongoli ya mikanda ya conception na Anglais eyambi.

### Mibeko ya moboko

- Nzela nyonso ezali relative — mosala esengeli kokokana na ba machines nionso. Nzela ya solo ya makasi epekisami.
- Kobakisa dépendance ya pip na module ya *motema* ya pipeline te. Biteni oyo ezali obligatoire te ekoki kosalela ba paquets oyo ezali obligatoire te, mpe esengeli kosala malamu ata soki ezangi.
- Kolemba machine ya état oyo ekendeke kaka liboso te — yango nde ndelo ya mosolo.
- Kobakisa bilembo ya leta ya Amerika te, mpe kobakisa eloko moko te oyo ezongisaka makambo oyo elongolami na source.
- Mbongwana ya schema ya D1 etali ba fichiers **mibale**: `pipeline/lib/manifest_schema.sql` mpe `db/schema.sql`.
- Meyomeko na code ya sika. Ba messages ya conventional-commit.

Tanga `CLAUDE.md` mpe `docs/20260511/00-*` liboso, sima fungola issue mpo na kosolola likambo nyonso ya monene liboso ya PR.

