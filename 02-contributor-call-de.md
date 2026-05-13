# GitHub — Beitrag 2 von 3 · Aufruf an Mitwirkende / "good first issues"

**Verwendung:** eine angepinnte Discussion ("Contributing & good first issues") oder der Einstieg in eine CONTRIBUTING.md.
**Stichworte:** Open Source, Mitwirken, good first issue, i18n, Lokalisierung, OCR, Python, TypeScript, Vitest, pytest, Barrierefreiheit, UAP, offene Daten
**Links:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Zu ufolens.com beitragen

[ufolens.com](https://www.ufolens.com) verwandelt das [PURSUE-UAP-Archiv](https://www.war.gov/ufo) des U.S. War Department in eine durchsuchbare, mehrsprachige Plattform mit [öffentlicher API](https://www.ufolens.com/api/v1). Das Projekt besteht aus zwei Hälften — einer lokalen Python-Ingest-Pipeline (`pipeline/`) und einer TypeScript/Hono-Edge-App (`worker/`) — die sich an einer einzigen Schnittstelle treffen: einem publizierten SQL- + Asset-Bundle.

Für Beiträge braucht es keinerlei Cloud-Zugangsdaten. Die Kern-Module der Pipeline kommen mit der stdlib aus, und die Worker-Tests laufen gegen In-Memory-Storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # sollte komplett grün sein, kein pip install nötig

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Wo Hilfe besonders viel bringt

**i18n / Lokalisierung** — `worker/src/i18n/ui-strings.json` ist die Quelle der UI-Strings. Muttersprachliche Reviews jeder Nicht-Englisch-Locale sind sehr wertvoll: holprige maschinelle Ausgaben einfangen, RTL-/Layout-Probleme fixen, Edge Cases der Sprachverhandlung verbessern.

**OCR-Qualität** — bessere Vorverarbeitung alter maschinengeschriebener Scans vor dem OCR; ein Evaluations-Harness, das die Open-Source-Engine gegen den Tesseract-Fallback auf Beispielseiten vergleicht.

**Barrierefreiheit** — die gerenderten Seiten (`worker/src/render/`) gegen WCAG auditieren; die CSP ist strikt (kein `unsafe-inline`), Lösungen müssen sich in diesem Rahmen bewegen.

**API-Ergonomie** — `worker/src/routes/` — Paginierung, Filterung, OpenAPI-Beschreibung, Beispiel-Clients.

**Pipeline-Robustheit** — mehr Graceful-Degradation-Pfade, bessere Fortschrittsberichte, Edge Cases der Delta-Erkennung (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (Traditionelles Chinesisch; `00-*` ist das Inhaltsverzeichnis). Englische Übersetzungen der Design-Dokumente sind willkommen.

### Grundregeln

- Alle Pfade relativ — das Projekt muss zwischen Maschinen portabel sein. Keine hardgecodeten absoluten Pfade.
- Keine pip-Abhängigkeit zu einem *Kern*-Modul der Pipeline hinzufügen. Optionale Stufen dürfen optionale Pakete nutzen, müssen aber ohne sie elegant degradieren.
- Die Forward-only-Zustandsmaschine nicht abschwächen — sie ist die Kostendeckelung.
- Keine offiziellen US-Regierungs-Hoheitszeichen einführen und nichts hinzufügen, was Schwärzungen der Quelle rückgängig macht.
- Schema-Änderungen an D1 betreffen **zwei** Dateien: `pipeline/lib/manifest_schema.sql` und `db/schema.sql`.
- Tests zu neuem Code. Commit-Nachrichten im Conventional-Commits-Stil.

Bitte zuerst `CLAUDE.md` und `docs/20260511/00-*` lesen, dann ein Issue eröffnen, um strukturelle Punkte vor dem PR zu diskutieren.
