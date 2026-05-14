# GitHub — Pos 2 vaan 3 · Oproop veur biedrages / "good first issues"

**Te gebruke es:** 'n vastgepinde Discussie ("Biedrage & good first issues") of 'n intro veur CONTRIBUTING.md.
**Trefwäörd:** open source, biedrage, good first issue, i18n, lokalisatie, OCR, Python, TypeScript, Vitest, pytest, touwgenkelekheid, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Biedrage aon ufolens.com

[ufolens.com](https://www.ufolens.com) maak vaan 't [PURSUE UAP-archief](https://www.war.gov/ufo) vaan 't Amerikaans Oorlogsdepartement e doorzeukbaar, meertalig platform mèt 'n [publieke API](https://www.ufolens.com/api/v1). 't Besteit oet twie deile — 'n lokaal Python-opnamepipeline (`pipeline/`) en 'n TypeScript/Hono edge-app (`worker/`) — die sammekoume in ein interface: 'ne gepubliceerde SQL + assets-bundel.

Geer höb gein cloud-credentials nudeg um biedrages te levere. De kernmodules vaan de pipeline zien allein aofhenkelek vaan de stdlib en de Worker-teste drejje tege 'n in-memory opslaag.

### Installatie

```bash
# pipeline
python3 -m pytest pipeline/tests/          # zou alles greun mote zien, gein pip-installatie nudeg

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Boe hölp 't mies welkom is

**i18n / lokalisatie** — `worker/src/i18n/ui-strings.json` is de bron vaan UI-tekskes. 'n Controle door 'ne moojertaolspreker vaan welke neet-Ingelse landstaol daan ouch is vaan groete weerde: oonhaolege masjienoetveur corrigeren, RTL/layout-probleme oplosse, randgevalle in taoloonderhandeling verbetere.

**OCR-kwaliteit** — beter veurverwerking vaan aaj getypde scans veur OCR; e testeveldj dat de open-source-engine vergeliek mèt de Tesseract-fallback op veurbeeldpagina's.

**Touwgenkelekheid** — de gerenderde pagina's (`worker/src/render/`) controleert tege WCAG; de CSP is strik (gein `unsafe-inline`), dus oplossinge mote binne dat kader wèrke.

**API-ergonomie** — `worker/src/routes/` — paginering, filtering, OpenAPI-besjrieving, veurbeeldclients.

**Pipeline-robuustheid** — mier paajer veur 'sierlek trökvalle', beter veurtgaanksrapportering, randgevalle veur delta-detectie (`pipeline/lib/delta.py`).

**Doc's** — `docs/20260511/` (繁體中文; `00-*` is de index). Vertaolinge vaan de oontwerpdoc's nao 't Ingels zien wèlkome.

### Groondregele

- Alle paajer relatief — 't projek moot draagbaar zien tösse computers. Gein helgecodeerde absolute paajer.
- Veur gein pip-aofhenkelekheid touw aon 'n *kern*module vaan de pipeline. Optioneel fases mage optioneel pakkette gebruke en mote sierlek trökvalle zoondet ze.
- Verswak de allein-nao-veure-staotsmachine neet — dat is 't kosteplafong.
- Veur gein officieel logo's vaan de Amerikaanse euverheid in en veug niks touw wat redacties oet de bron oongedoon maak.
- D1-schemaveranderinge rake **twie** bestande: `pipeline/lib/manifest_schema.sql` en `db/schema.sql`.
- Teste mèt nujje code. Conventional-commit-berichte.

Lees iers `CLAUDE.md` en `docs/20260511/00-*`, en eupen daan 'n issue um get structureels te bespreke veurtot geer 'ne PR indient.

