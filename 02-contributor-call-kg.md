# GitHub — Nsangu 2 ya 3 · Kubokila bantu ya ke sadisaka / "mambu ya mbote ya kuyantika"

**Sadila bonso:** mosi ya Discussion ya bo me kangisa ("Kusadisa & mambu ya mbote ya kuyantika") to ntwadisi ya CONTRIBUTING.md.
**Mvovo ya mfunu:** open source, kusadisa, mambu ya mbote ya kuyantika, i18n, kunata na ndinga ya kisika, OCR, Python, TypeScript, Vitest, pytest, kunata na bantu ya kele ti bampasi, UAP, data ya me fionguna
**Bikwati:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kusadisa na ufolens.com

[ufolens.com](https://www.ufolens.com) ke sobaka [arsiv ya PURSUE UAP](https://www.war.gov/ufo) ya Departema ya Vita ya États-Unis na n'kua ya kusosa, ya bandinga mingi ti [API ya kimvwama](https://www.ufolens.com/api/v1). Yo kele bitini zole — pipeline ya kubaka bima ya Python ya kisika (`pipeline/`) mpi programe ya nsongi ya TypeScript/Hono (`worker/`) — ya ke kutanaka na interface mosi: kitini ya SQL + bima ya bo me basisa.

Nge kele ve na mfunu ya ata mikanda ya kusonika ya matata sambu na kusadisa. Ba modules ya ntima ya pipeline kele ya stdlib mpamba mpi ba meko ya Worker ke kwendaka na kubumba ya kati.

### Kuyidika

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kisika lusadisu kele ya mfunu mingi

**i18n / kunata na ndinga ya kisika** — `worker/src/i18n/ui-strings.json` kele kisina ya bangogo ya UI. Kutala ya muntu ya kele ndinga na yandi ya kibutuka ya konso ndinga ya kele ve Kingelesi kele ya mfunu mingi: kubaka bangogo ya masini ya kele ve mbote, kuyidika mambu ya RTL/ya kubasisa, kuyidika mambu ya mpasi ya nkubu ya ndinga.

**Bondeko ya OCR** — kuyidika mbote mikanda ya ntama ya bo me sonikaka na masini na ntwala ya OCR; programe ya kumeka ya ke sosaka nsasa kati ya motere ya me fionguna mpi Tesseract na balupangu ya kumeka.

**Kunata na bantu ya kele ti bampasi** — tala balupangu ya bo me basisa (`worker/src/render/`) na kuwakana ti WCAG; CSP kele ya ngolo (ata `unsafe-inline` ve), kansi ba nzila ya kusala fwete wakana ti yo.

**Kusadila API mbote** — `worker/src/routes/` — kuyidika balupangu, kufiltra, nsunzula ya OpenAPI, ba programe ya kumeka.

**Ngolo ya Pipeline** — nzila mingi ya kulanda kusala mbote, kulonga mbote kima ya ke salamaka, mambu ya mpasi ya kumona bansoba (`pipeline/lib/delta.py`).

**Mikanda** — `docs/20260511/` (繁體中文; `00-*` kele index). Kubalula mikanda ya plan na Kingelesi kele ya kuluta mbote.

### Bansiku ya ntoto

- Banzila yonso kele ya kuwakana — proje fwete kwenda na masini yonso. Ata nzila mosi ya bo me sonikaka na ngolo fwete bwa ve.
- Kudika ve ata dependance ya pip na module ya *ntima* ya pipeline. Bitini ya bo ke ponaka lenda sadila bapaketi ya bo ke ponaka, mpi fwete landa kusala mbote kana yo kele ve.
- Lemba ve masini ya ke kwendaka kaka na ntwala — yo yina kele ndilu ya kufuta.
- Tula ve bidimbu ya luyalu ya États-Unis, mpi tula ve ata kima mosi ya ke vutulaka mikanda ya bo me fukisa.
- Bansoba ya schema ya D1 ke simba ba fisie **zole**: `pipeline/lib/manifest_schema.sql` mpi `db/schema.sql`.
- Meko ti code ya mpa. Nsangu ya kusoba na kuwakana ti bansiku.

Tanga `CLAUDE.md` mpi `docs/20260511/00-*` na ntwala, na nima kangula diambu sambu na kusolula konso kima ya nene na ntwala ya PR.
