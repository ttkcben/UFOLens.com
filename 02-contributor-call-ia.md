# GitHub — Publicacion 2 de 3 · Appello a contributor / "bon prime problemas"

**Usar como:** un Discussion affixate ("Contribution e bon prime problemas") o un introduction a CONTRIBUTING.md.
**Parolas clave:** codice aperte, contribution, bon prime problema, i18n, localisation, OCR, Python, TypeScript, Vitest, pytest, accessibilitate, UAP, datos aperte
**Hyperligamines:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuer a ufolens.com

[ufolens.com](https://www.ufolens.com) transforma le [archivo PURSUE UAP](https://www.war.gov/ufo) del Departimento de Guerra del S.U. in un platteforma perquisibile e multilingue con un [API public](https://www.ufolens.com/api/v1). Illo ha duo medietates — un pipeline de ingestion local in Python (`pipeline/`) e un app al bordo in TypeScript/Hono (`worker/`) — que se incontra a un interfacie: un pacchetto publicate de SQL + activos.

Tu non necessita alcun credential del nube pro contribuer. Le modulos central del pipeline es solmente del bibliotheca standard e le tests del Worker se executa contra un immagazinage in memoria.

### Installation

```bash
# pipeline
python3 -m pytest pipeline/tests/          # tote debe esser verde, nulle installation de pip necessari

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ubi le adjuta es le plus utile

**i18n / localisation** — `worker/src/i18n/ui-strings.json` es le fonte del catenas de texto del interfacie de usator. Le revision per un parlatore native de qualcunque localitate non-anglese es de alte valor: deteger resultatos machinal estranie, corriger problemas de RTL/disposition, e meliorar casos marginal de negotiation de lingua.

**Qualitate del OCR** — melior pre-processamento de vetule scansiones dactylographate ante le OCR; un harnese de evalutation que compara le motor de codice aperte con le alternativa Tesseract sur paginas de exemplo.

**Accessibilitate** — auditar le paginas rendite (`worker/src/render/`) contra WCAG; le CSP es stricte (non `unsafe-inline`), dunque le solutiones debe functionar intra iste limitation.

**Ergonomia del API** — `worker/src/routes/` — pagination, filtration, description OpenAPI, exemplos de clientes.

**Robustessa del pipeline** — plus de vias de degradation gratiose, melior reportage de progresso, casos marginal in le detection de deltas (`pipeline/lib/delta.py`).

**Documentos** — `docs/20260511/` (繁體中文; `00-*` es le indice). Traductiones del documentos de designo a in anglese es benvenite.

### Regulas de base

- Tote le camminos es relative — le projecto debe esser portabile inter machinas. Nulle camminos absolute codificate.
- Non adde un dependentia pip a un modulo central del pipeline. Phases optional pote usar pacchettos optional, e debe degradar se gratiosemente sin illos.
- Non indebili le machina de statos a progression solmente — isto es le plafon de costo.
- Non introduce insignias official del governamento del S.U., e non adde alique que reverte le redactiones del fonte.
- Cambios al schema de D1 tocca **duo** files: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Tests con nove codice. Messages de `Conventional Commits`.

Lege `CLAUDE.md` e `docs/20260511/00-*` primo, postea aperi un problema pro discuter qualcunque cosa structural ante le PR.
