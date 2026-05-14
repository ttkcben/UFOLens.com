# GitHub — Beitrag 2 vo 3 · Aufruaf zur Mithuif / "guade erste Afgobn"

**Vawendung:** Ois a ogheftete Diskussion ("Mitwirkn & guade erste Afgobn") oder a Einleitung fia `CONTRIBUTING.md`.
**Schlisslwörter:** Open Source, mitwirkn, guade erste Afgob, i18n, Lokalisierung, OCR, Python, TypeScript, Vitest, pytest, Barrierefreiheit, UAP, offane Datn
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mitwirkn bei ufolens.com

[ufolens.com](https://www.ufolens.com) mocht s [PURSUE UAP-Archiv](https://www.war.gov/ufo) vom U.S. War Department zu ara suachborn, mehrsprochign Plattform mit am [effentlichn API](https://www.ufolens.com/api/v1). S'san zwoa Hälftn — a lokale Python Ingest-Pipeline (`pipeline/`) und a TypeScript/Hono Edge-App (`worker/`) — de si an oana Schnittstej treffn: am vaeffentlichtn SQL + Assets-Bündl.

Du brauchst koane Cloud-Zugangsdatn, um mitzwirkn. De Kernmodule vo da Pipeline san stdlib-only und de Worker-Tests laffn gegn an In-Memory-Speicha.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # soit ois grean sei, koa pip install nedig

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Wo Hinf am meistn bringt

**i18n / Lokalisierung** — `worker/src/i18n/ui-strings.json` is de Quelln fia de UI-Texte. A Iwaorbeitung vo am Muadasprochla fia jede ned-englische Lokale is vui wert: komische maschinelle Ausgabn findn, RTL/Layout-Problem beheben, Sprochvahandlungs-Grenzfäll vabessan.

**OCR-Qualität** — bessare Vorvaoabeitung vo oide tipptate Scans vor da OCR; a Evaluierungs-Framework, des de Open-Source-Engine mitm Tesseract-Fallback auf Beispieseitn vagleicht.

**Barrierefreiheit** — de rendertn Seitn (`worker/src/render/`) gegn WCAG prüfn; s CSP is strikt (koa `unsafe-inline`), deshoib miassn Lösunga damit funktionian.

**API-Ergonomie** — `worker/src/routes/` — Seitnumbruch, Filta, OpenAPI-Beschreibung, Beispui-Clients.

**Pipeline-Robustheit** — mehr Pfade fia a saubas Funktionian bei Fehlern, bessas Fortschritts-Reporting, Grenzfälle bei da Delta-Erkennung (`pipeline/lib/delta.py`).

**Doks** — `docs/20260511/` (繁體中文; `00-*` is da Index). Ibasetzunga vo de Design-Doks ins Englische san willkomma.

### Grundregln

- Alle Pfade relativ — s Projekt muass portierbor zwischn Rechna sei. Koane fix eincodiertn absoluten Pfade.
- Fia a Pipeline-*Kern*-Modul koa pip-Abhängigkeit hinzufügn. Optionale Stufn kinna optionale Pakete vawendn und miassn a ohne de a sauba funktionian.
- De "forward-only"-Zustandsmaschin ned aufweichn — de is da Kostndeckl.
- Koane offizielln US-Regiarungsobzeichn hinzufügn, und a nix, was Quelln-Schwärzunga rückgängig mocht.
- D1-Schema-Änderunga betreffen **zwoa** Datein: `pipeline/lib/manifest_schema.sql` und `db/schema.sql`.
- Tests mit neim Code. Conventional-Commit-Nochrichtn.

Lies z'erst `CLAUDE.md` und `docs/20260511/00-*`, dann moch a Issue auf, um strukturelle Sochn z'besprechn, bevorst an PR aufmochst.

