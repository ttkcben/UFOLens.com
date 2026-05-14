# GitHub — Pos 2 van 3 · Oproep vir bydraers / "goeie eerste kwessies"

**Gebruik as:** 'n vasgepende Bespreking ("Bydraes & goeie eerste kwessies") of 'n `CONTRIBUTING.md`-inleiding.
**Sleutelwoorde:** oopbron, bydra, goeie eerste kwessie, i18n, lokalisering, OCR, Python, TypeScript, Vitest, pytest, toeganklikheid, UAP, oop data
**Hipskakels:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Dra by tot ufolens.com

[ufolens.com](https://www.ufolens.com) omskep die V.S. Oorlogsdepartement se [PURSUE UAP-argief](https://www.war.gov/ufo) in 'n soekbare, meertalige platform met 'n [openbare API](https://www.ufolens.com/api/v1). Dit bestaan uit twee helftes — 'n plaaslike Python-innamepyplyn (`pipeline/`) en 'n TypeScript/Hono-randtoepassing (`worker/`) — wat by een koppelvlak ontmoet: 'n gepubliseerde SQL + bates-bundel.

Jy het geen wolk-bewyse nodig om by te dra nie. Die pyplyn se kernmodules is slegs-stdlib en die Worker-toetse loop teen in-geheue-berging.

### Opstelling

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Waar hulp die nuttigste is

**i18n / lokalisering** — `worker/src/i18n/ui-strings.json` is die bron van UI-stringe. Hersiening deur 'n moedertaalspreker van enige nie-Engelse lokaal is van hoë waarde: vang ongemaklike masjienuitvoer op, stel RTL/uitlegkwessies reg, verbeter randgevalle van taalonderhandeling.

**OCR-kwaliteit** — beter voorverwerking van ou getikte skanderings voor OCR; evalueringsharnas wat die oopbron-enjin vergelyk met die Tesseract-terugval op voorbeeldbladsye.

**Toeganklikheid** — oudit die weergegee bladsye (`worker/src/render/`) teen WCAG; die CSP is streng (geen `unsafe-inline`), so oplossings moet daarbinne werk.

**API-ergonomie** — `worker/src/routes/` — paginering, filtrering, OpenAPI-beskrywing, voorbeeldkliënte.

**Pyplyn-robuustheid** — meer grasieuse afgraderingspaaie, beter vorderingsverslaggewing, randgevalle in delta-opsporing (`pipeline/lib/delta.py`).

**Dokumente** — `docs/20260511/` (繁體中文; `00-*` is die indeks). Vertalings van die ontwerp-dokumente na Engels is welkom.

### Grondreëls

- Alle paaie relatief — die projek moet draagbaar wees oor masjiene heen. Geen hardgekodeerde absolute paaie nie.
- Moenie 'n pip-afhanklikheid by 'n pyplyn *kern*-module voeg nie. Opsionele stadia mag opsionele pakkette gebruik, en moet grasieus afgradeer daarsonder.
- Moenie die slegs-vorentoe-toestandmasjien verswak nie — dit is die kosteplafon.
- Moenie amptelike V.S. regeringsinsignia byvoeg nie, en moenie enigiets byvoeg wat bronredaksies omkeer nie.
- D1-skemaveranderinge raak **twee** lêers: `pipeline/lib/manifest_schema.sql` en `db/schema.sql`.
- Toetse met nuwe kode. Konvensionele-commit boodskappe.

Lees eers `CLAUDE.md` en `docs/20260511/00-*`, maak dan 'n kwessie oop om enigiets struktureels te bespreek voor die PR.

