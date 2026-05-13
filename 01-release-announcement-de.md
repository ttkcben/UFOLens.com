# GitHub — Beitrag 1 von 3 · Release-/README-Ankündigungsblock

**Verwendung:** als GitHub-Release-Text, angepinnte Discussion oder Kopfteil der Repo-README.
**Stichworte:** UAP, UFO, PURSUE-Archiv, deklassifizierte Dokumente, offene Daten, Volltextsuche, OCR, maschinelle Übersetzung, lokales LLM, Ollama, Edge-Computing, öffentliche API, Hono, TypeScript, Python
**Links:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — eine mehrsprachige, durchsuchbare Plattform für das PURSUE-UAP-Archiv

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Quellarchiv:** https://www.war.gov/ufo

`ufolens.com` veröffentlicht das **PURSUE**-Archiv des U.S. War Department mit deklassifizierten UAP-/UFO-Akten neu — als Wissensplattform: Volltextsuche über das gesamte Corpus, maschinelle Übersetzung, Erkundung per Karte + Zeitleiste und eine öffentliche JSON-API. Die Quelldokumente sind Werke der US-Bundesregierung und innerhalb der USA gemeinfrei ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dieses Projekt **ist nicht mit der US-Regierung verbunden**, verwendet keinerlei offizielle Hoheitszeichen und macht Schwärzungen niemals rückgängig.

### Architektur

```
Lokale Maschine (Apple Silicon, Heim-IP)             Edge-Netzwerk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, Kern nur stdlib)            worker/  (TypeScript, Hono.js)
  fetch → OCR → übersetzen → publish (forward-only)    /{lang}/...   Seiten
  OCR: Open-Source-Engine (Tesseract CLI als Fallback) /api/v1/...   öffentliche API
  Übersetzen / NER: lokales LLM (Gemma via Ollama)     /admin        Betriebskonsole
  Status: SQLite-Manifest                            Unterbau: Edge SQL DB, Objekt-
        │                                              Speicher (Original-PDFs), KV-Cache
        └── publiziertes Bundle: SQL + Asset-Manifest + Purge-Liste ──┘
```

- **Null Cloud-AI-Kosten pro Dokument.** OCR und Übersetzung laufen lokal; die Forward-only-Zustandsmaschine (`discovered → downloaded → ocr_done → translated → published`) garantiert, dass ein Dokument nicht neu verarbeitet wird, solange es sich nicht geändert hat.
- **Der Pipeline-Kern hat keine Drittabhängigkeiten** — Parsing-/Manifest-/Delta-Module laufen und testen mit einem frischen Python ohne jegliches `pip install`; OCR-/Übersetzungsstufen degradieren elegant, wenn optionale Pakete fehlen.
- **Die Edge-Site** setzt strikte Security-Header + CSP (kein `unsafe-inline`; inline JSON-LD ist per sha256 gepinnt), Sprachverhandlung via `Accept-Language` + Länder-Mapping, einen 30-Tage-KV-Seiten-Cache und einen täglichen Housekeeping-Cron.
- **Inkrementelle Updates:** ein Delta-Detector diff-t den Quellindex und reicht nur Änderungen zurück in die Pipeline.

### Für Entwickler

Die öffentliche API unter https://www.ufolens.com/api/v1 liefert Dokumente und Metadaten als JSON. Anonymer Zugriff ist rate-limitiert; einen Key für Researcher-/Developer-Tier bitte anfragen. Endpoints und Limits siehe API-Sektion auf der Site.

### Status

Code fertig; Site deployed auf https://www.ufolens.com. Die Produktionsdatenbank wird befüllt, indem die Offline-Pipeline läuft und das Bundle vorwärts publiziert wird (`cli_publish run --remote`). Die vollständigen Design-Docs liegen in `docs/20260511/`.

### Lizenz / Grenzen

- Quelldokumente: Werke der US-Bundesregierung, gemeinfrei innerhalb der USA.
- Der eigene Code dieser Plattform: siehe `LICENSE`.
- Die Site sendet `Tdm-Reservation: 1` und `X-Robots-Tag: noai, noimageai` — indexierbar durch Suchmaschinen, Opt-out gegenüber KI-Training/-Scraping.
- Videomaterial wird DVIDS / AARO zugeordnet und nicht von diesem Projekt beansprucht.

Issues und PRs sind willkommen. Bitte `CLAUDE.md` und `docs/20260511/00-*` lesen, bevor strukturelle Änderungen vorgeschlagen werden.
