# GitHub — Nuntius II ex III · Vocatio ad contributores / "prima problemata bona"

**Uti ut:** disputatio fixa ("Contributio & prima problemata bona") aut introductio ad CONTRIBUTING.md.
**Claves verborum:** fons apertus, contributio, primum problema bonum, i18n, localizatio, OCR, Python, TypeScript, Vitest, pytest, accessibilitas, UAP, data aperta
**Hypertextus:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## De contributione ad ufolens.com

[ufolens.com](https://www.ufolens.com) [archivum PURSUE UAP](https://www.war.gov/ufo) a Departmento Belli Civitatum Foederatarum vertit in suggestum pervestigabile et multilingue cum [API publico](https://www.ufolens.com/api/v1). Duae partes sunt — catena operum Python localis (`pipeline/`) et applicatio in margine TypeScript/Hono (`worker/`) — quae in una interface conveniunt: fasciculo publicato SQL + bonorum.

Non eges ullis documentis nubis ad contribuendum. Moduli nucleares catenae operum stdlib-tantum sunt et probationes pro Worker contra repositorium in memoria currunt.

### Praeparatio

```bash
# pipeline
python3 -m pytest pipeline/tests/          # omnia viridia esse debent, nullus pip install necessarius

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ubi auxilium utilissimum est

**i18n / localizatio** — `worker/src/i18n/ui-strings.json` est fons catenarum interfaciei utentis. Recensio ab oratore nativo cuiusvis localis non-Anglici magni pretii est: deprehende translationes machinales incommodas, corrige problemata RTL/dispositionis, meliora casus marginales negotiationis linguae.

**Qualitas OCR** — melior prae-processus veterum scan-orum dactylographiatorum ante OCR; instrumentum evaluationis quod machinam fontis aperti cum subsidiario Tesseract in paginis exempli comparat.

**Accessibilitas** — audita paginas redditas (`worker/src/render/`) contra WCAG; CSP strictum est (nullum `unsafe-inline`), ergo solutiones intra hoc operari debent.

**Ergonomia API** — `worker/src/routes/` — paginatio, filtratio, descriptio OpenAPI, clientes exemplares.

**Robur catenae operum** — plures viae degradationis gratiosae, melior relatio progressus, casus marginales detectionis differentiarum (`pipeline/lib/delta.py`).

**Documenta** — `docs/20260511/` (繁體中文; `00-*` est index). Translationes documentorum designationis in Anglicum gratae sunt.

### Regulae fundamentales

- Omnes viae relativae — consilium portabile esse debet per machinas. Nullae viae absolutae hardcoded.
- Noli addere dependentiam pip ad modulum *nuclearem* catenae operum. Stadia optionalia fasciculis optionalibus uti possunt, et gratiose degradari debent sine eis.
- Noli debilitare machinam statuum quae solum progreditur — hoc est tectum sumptuum.
- Noli introducere insignia officialia gubernationis Civitatum Foederatarum, et noli addere quicquam quod redactiones fontis invertat.
- Mutationes schematis D1 **duos** fasciculos tangunt: `pipeline/lib/manifest_schema.sql` et `db/schema.sql`.
- Probationes cum novo codice. Nuntii commit secundum Conventional Commits.

Lege `CLAUDE.md` et `docs/20260511/00-*` primum, deinde aperi problema ad disputandum de quavis re structurali ante PR.

