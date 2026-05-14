# GitHub — Post 2 van 3 · Oproep voor bijdragen / "good first issues"

**Gebruik als:** een vastgezette Discussion ("Bijdragen & good first issues") of een intro voor CONTRIBUTING.md.
**Trefwoorden:** open source, bijdragen, good first issue, i18n, lokalisatie, OCR, Python, TypeScript, Vitest, pytest, toegankelijkheid, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bijdragen aan ufolens.com

[ufolens.com](https://www.ufolens.com) transformeert het [PURSUE UAP-archief](https://www.war.gov/ufo) van het Amerikaanse Ministerie van Oorlog in een doorzoekbaar, meertalig platform met een [publieke API](https://www.ufolens.com/api/v1). Het bestaat uit twee helften — een lokale Python-ingest-pipeline (`pipeline/`) en een TypeScript/Hono edge-app (`worker/`) — die samenkomen bij één interface: een gepubliceerde bundel van SQL + assets.

Je hebt geen cloud-credentials nodig om bij te dragen. De kernmodules van de pipeline zijn stdlib-only en de tests van de Worker draaien tegen in-memory opslag.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # zou allemaal groen moeten zijn, geen pip install nodig

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Waar hulp het meest nuttig is

**i18n / lokalisatie** — `worker/src/i18n/ui-strings.json` is de bron voor UI-strings. Een review door een moedertaalspreker van elke niet-Engelse locale is van grote waarde: spoor ongemakkelijke machinevertalingen op, los RTL/layout-problemen op, verbeter edge cases bij taalnegotiatie.

**OCR-kwaliteit** — betere voorbewerking van oude getypte scans voor OCR; een evaluatie-harnas dat de open-source engine vergelijkt met de Tesseract-fallback op voorbeelpagina's.

**Toegankelijkheid** — audit de gerenderde pagina's (`worker/src/render/`) tegen WCAG; de CSP is strikt (geen `unsafe-inline`), dus oplossingen moeten daarbinnen werken.

**API-ergonomie** — `worker/src/routes/` — paginering, filtering, OpenAPI-beschrijving, voorbeeldclients.

**Pipeline-robuustheid** — meer paden voor graceful degradation, betere voortgangsrapportage, edge cases voor delta-detectie (`pipeline/lib/delta.py`).

**Documentatie** — `docs/20260511/` (繁體中文; `00-*` is de index). Vertalingen van de ontwerpdocumenten naar het Engels zijn welkom.

### Basisregels

- Alle paden relatief — het project moet overdraagbaar zijn tussen machines. Geen hardgecodeerde absolute paden.
- Voeg geen pip-afhankelijkheid toe aan een *core*-module van de pipeline. Optionele stadia mogen optionele packages gebruiken, en moeten graceful degraderen zonder.
- Verzwak de 'forward-only' state machine niet — dat is het kostenplafond.
- Introduceer geen officiële insignes van de Amerikaanse overheid, en voeg niets toe dat redacties in de bron ongedaan maakt.
- D1-schemawijzigingen raken **twee** bestanden: `pipeline/lib/manifest_schema.sql` en `db/schema.sql`.
- Tests bij nieuwe code. Conventional-commit-berichten.

Lees eerst `CLAUDE.md` en `docs/20260511/00-*`, en open dan een issue om structurele zaken te bespreken voordat je een PR opent.
