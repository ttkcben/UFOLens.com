# GitHub — Pos 2 van 3 · Oproep vir bydraers / "goeie eerste kwessies"

**Gebruik as:** 'n vasgespelde Bespreking ("Bydraes & goeie eerste kwessies") of 'n inleiding tot CONTRIBUTING.md.
**Sleutelwoorde:** oopbron, bydraes, goeie eerste kwessie, i18n, lokalisering, OCR, Python, TypeScript, Vitest, pytest, toeganklikheid, UAP, oop data
**Hipskakels:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Dra by tot ufolens.com

[ufolens.com](https://www.ufolens.com) omskep die V.S. Departement van Oorlog se [PURSUE UAP-argief](https://www.war.gov/ufo) in 'n soekbare, veeltalige platform met 'n [openbare API](https://www.ufolens.com/api/v1). Dit is twee helftes — 'n plaaslike Python-innamepyplyn (`pipeline/`) en 'n TypeScript/Hono-randtoepassing (`worker/`) — wat by een koppelvlak ontmoet: 'n gepubliseerde SQL + bates-bundel.

Jy het geen wolk-geloofsbriewe nodig om by te dra nie. Die pyplyn se kernmodules is slegs-stdlib en die Worker-toetse loop teen in-geheue-berging.

### Opstelling

```bash
# pyplyn
python3 -m pytest pipeline/tests/          # behoort alles groen te wees, geen pip-installasie nodig nie

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Waar hulp die nuttigste is

**i18n / lokalisering** — `worker/src/i18n/ui-strings.json` is die bron van UI-stringe. Moedertaalspreker-hersiening van enige nie-Engelse lokaliteit is van hoë waarde: vang lomp masjienuitsette op, los RTL/uitlegkwessies op, verbeter randgevalle van taalonderhandeling.

**OCR-kwaliteit** — beter voorverwerking van ou, getikte skanderings voor OCR; evaluasieharnas wat die oopbron-enjin vergelyk met die Tesseract-terugval op voorbeeldbladsye.

**Toeganklikheid** — oudit die gelewerde bladsye (`worker/src/render/`) teen WCAG; die CSP is streng (geen `unsafe-inline`), so oplossings moet daarbinne werk.

**API-ergonomie** — `worker/src/routes/` — paginering, filtrering, OpenAPI-beskrywing, voorbeeldkliënte.

**Pyplyn-robuustheid** — meer paaie vir grasieuse afgradering, beter vorderingsverslaggewing, delta-opsporing randgevalle (`pipeline/lib/delta.py`).

**Dokumentasie** — `docs/20260511/` (繁體中文; `00-*` is die indeks). Vertalings van die ontwerpdokumente na Engels is welkom.

### Grondreëls

- Alle paaie relatief — die projek moet oordraagbaar wees tussen masjiene. Geen hardgekodeerde absolute paaie nie.
- Moenie 'n pip-afhanklikheid by 'n pyplyn-*kern*-module voeg nie. Opsionele stadia mag opsionele pakkette gebruik, en moet grasieus afgradeer daarsonder.
- Moenie die slegs-vorentoe-toestandsmasjien verswak nie — dit is die kosteplafon.
- Moenie amptelike V.S. regeringskentekens byvoeg nie, en moenie enigiets byvoeg wat bronredaksies ongedaan maak nie.
- D1-skemaveranderinge raak **twee** lêers: `pipeline/lib/manifest_schema.sql` en `db/schema.sql`.
- Toetse met nuwe kode. Konvensionele-commit-boodskappe.

Lees eers `CLAUDE.md` en `docs/20260511/00-*`, en open dan 'n kwessie om enigiets struktureels te bespreek voor die PR.

