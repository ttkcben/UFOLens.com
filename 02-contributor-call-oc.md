# GitHub — Publicacion 2 de 3 · Apèl a contribucions / "bons primièrs issues"

**Utilizar coma:** una discussion apngada ("Contribucions e bons primièrs issues") o una introduccion a CONTRIBUTING.md.
**Mots clau:** open source, contribucion, bon primièr issue, i18n, localizacion, OCR, Python, TypeScript, Vitest, pytest, accessibilitat, UAP, donadas obèrtas
**Iperligams:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuir a ufolens.com

[ufolens.com](https://www.ufolens.com) transforma las [archius UAP PURSUE](https://www.war.gov/ufo) del Departament de la Guèrra dels EUA en una plataforma cercabla e multilingüa amb una [API publica](https://www.ufolens.com/api/v1). Es compausat de doas mitats — un pipeline d'ingestion Python local (`pipeline/`) e una aplicacion Edge TypeScript/Hono (`worker/`) — que se reünisson a una sola interfàcia: un paquet SQL + actius publicat.

Avètz pas besonh de credencials cloud per contribuir. Los moduls principals del pipeline son unicament stdlib e los tèsts del Worker s'executan contra un estocatge en memòria.

### Installacion

```bash
# pipeline
python3 -m pytest pipeline/tests/          # deuriá èsser tot verd, pas de pip install necessari

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ont l'ajuda es mai utila

**i18n / localizacion** — `worker/src/i18n/ui-strings.json` es la font de las cadenas de l'interfàcia utilizaire. La revision per un locutor natiu de tota localizacion non anglesa a una granda valor: detectar de resultats automatics estranhs, corregir de problèmas de RTL/disposicion, melhorar los cases limitas de negociacion de lenga.

**Qualitat de l'OCR** — melhor pretractament de las vielhas numerizacions dactilografiadas abans l'OCR; arnés d'evaluacion comparant lo motor open-source amb la solucion de recors Tesseract sus de paginas d'exemple.

**Accessibilitat** — auditar las paginas rendudas (`worker/src/render/`) contra WCAG; lo CSP es estricte (pas de `unsafe-inline`), doncas las solucions devon foncionar dins aquel quadre.

**Ergonomia de l'API** — `worker/src/routes/` — paginacion, filtratge, descripcion OpenAPI, clients d'exemple.

**Robustesa del pipeline** — mai de camins de degradacion eleganta, melhor rapòrt de progression, cases limitas de deteccion de deltas (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` es l'indèx). Las traduccions dels documents de concepcion en anglés son benvengudas.

### Règlas de basa

- Totes los camins relatius — lo projècte deu èsser portable entre las maquinas. Pas de camins absoluts codats en dur.
- Ajustetz pas de dependéncia pip a un modul *principal* del pipeline. Las estapas opcionalas pòdon utilizar de paquets opcionals, e devon se degradar elegantament sens eles.
- Aflaquissètz pas la maquina d'estats d'avançament unic — es lo plafon de còst.
- Introduisètz pas d'insignes oficials del govèrn dels EUA, e ajustetz res que pòsca anullar las redaccions de la font.
- Los cambiaments d'esquèma D1 afèctan **dos** fichièrs: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- De tèsts amb lo còdi novèl. De messatges de commit convencionals.

Legissètz d'en primièr `CLAUDE.md` e `docs/20260511/00-*`, puèi obrissètz un issue per discutir de tot çò estructural abans lo PR.
