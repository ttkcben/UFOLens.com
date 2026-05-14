# GitHub — Biitrag 2 vo 3 · Uffruef für Mitwirkendi / "gueti erschti Issues"

**Verwendig als:** e fixierti Diskussion ("Mitwirke & gueti erschti Issues") oder als Intro für `CONTRIBUTING.md`.
**Schlüsselwörter:** Open Source, Mitwirke, gueti erschti Issue, i18n, Lokalisierig, OCR, Python, TypeScript, Vitest, pytest, Zuegänglichkeit, UAP, offeni Date
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mitwirke bi ufolens.com

[ufolens.com](https://www.ufolens.com) verwandlet s [PURSUE UAP-Archiv](https://www.war.gov/ufo) vom US-Kriegsministerium in e suechbari, mehrschpraachigi Plattform mitere [öffentliche API](https://www.ufolens.com/api/v1). Es sind zwei Hälftene – e lokali Python Ingest-Pipeline (`pipeline/`) und e TypeScript/Hono Edge-App (`worker/`) – wo sich a einere Schnittstell treffed: emene veröffentlichte SQL + Assets Bundle.

Du bruchsch keini Cloud-Zuegangsdate zum Mithelfe. D'Chärnmodul vo de Pipeline sind stdlib-only und d'Worker-Tests laufe gäge en In-Memory-Speicher.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # sött alles grüen si, keis pip install nötig

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Wo Hilf am meischte nützt

**i18n / Lokalisierig** — `worker/src/i18n/ui-strings.json` isch d'Quälle für d'UI-Strings. E Überprüefig vo jedem nöd-englische Locale durch en Muetterschpraachler isch sehr wertvoll: unpassendi maschinelli Usgääb finde, RTL/Layout-Problem behebe, Randfäll bi de Schpraachverhandlig verbessere.

**OCR-Qualität** — besseri Vorverarbeitig vo alte, schriibmaschinegschribnige Scans vor de OCR; e Bewertigs-Harness, wo d'Open-Source-Engine mit em Tesseract-Fallback uf Biispielsiite vergliicht.

**Zuegänglichkeit** — d'renderte Siite (`worker/src/render/`) gäge WCAG prüfe; d'CSP isch schträng (keis `unsafe-inline`), drum müend Lösige innerhalb vo dem Rahme funktioniere.

**API-Ergonomie** — `worker/src/routes/` — Paginierig, Filterig, OpenAPI-Beschriibig, Biispiel-Clients.

**Pipeline-Robustheit** — meh würdevoui Degradierigs-Pfad, bessers Fortschritts-Reporting, Randfäll bi de Delta-Erkennig (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` isch de Index). Übersetzige vo de Design-Dokumänt uf Englisch sind willkomme.

### Grundregle

- Alli Pfad relativ – s'Projekt muess über Maschine portabel si. Keini hartcodierte absolute Pfad.
- Füeg keini pip-Abhängigkeit zu eme Pipeline-*Chärn*-Modul dezue. Optionale Stufe dörfed optionali Paket verwende und müend würdevoll degradier, wenn die fehled.
- Schwäch d'vorwärtsgerichtet Zustandsmaschiine nöd ab – das isch d'Choschtdeckeni.
- Füeg keini offizielle US-Regierigs-Abzeiche dezue und nüt, wo d'Schwärzige vo de Quälle rückgängig macht.
- D1-Schemaänderige beträffed **zwei** Dateie: `pipeline/lib/manifest_schema.sql` und `db/schema.sql`.
- Tests mit neuem Code. Conventional-Commit-Nachrichte.

Lies zersch `CLAUDE.md` und `docs/20260511/00-*`, denn mach en Issue uf, um öpis Strukturells z'diskutiere, bevor de PR chunnt.
