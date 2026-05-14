# GitHub — Biitrag 1 vo 3 · Release / README Aakündigungsblock

**Verwendig als:** GitHub Release-Body, e fixierti Diskussion oder zu Oberscht im Repo-README.
**Schlüsselwörter:** UAP, UFO, PURSUE-Archiv, freigähni Dokumänt, offeni Date, Volltext-Suech, OCR, maschinelli Übersetzig, lokals LLM, Ollama, Edge Computing, öffentischi API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — e mehrschpraachigi, suechbari Plattform für s PURSUE UAP-Archiv

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Quell-Archiv:** https://www.war.gov/ufo

`ufolens.com` veröffentlicht s **PURSUE**-Archiv vom US-Kriegsministerium mit freigähne UAP / UFO-Aktene als Wissensplattform neu: Volltext-Suech, maschinelli Übersetzig über de ganzi Corpus, Charte- + Ziitstrahl-Erkundig und e öffentischi JSON-API. D'Quälledokumänt sind Werke vo de US-Bundesregierig und sind innerhalb vo de USA Public Domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Das Projekt isch **nöd mit de US-Regierig verbunde**, verwendet keini offizielle Abzeiche und macht niemals Schwärzige rückgängig.

### Architektur

```
Lokale Maschine (Apple Silicon, Heimet-IP)       Edge-Netzwärch
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   Siite
  OCR: Open-Source-Engine (Tesseract CLI Fallback)     /api/v1/...   öffentischi API
  translate / NER: lokals LLM (Gemma via Ollama)       /admin        Betriiberkonsoli
  state: SQLite manifest                             understützt durch: Edge SQL DB,
        │                                              Objektspeicher (Quell-PDFs), KV-Cache
        └── veröffentlicht es Bundle: SQL + Asset-Manifest + Cache-Löschliste ──┘
```

- **Null Pro-Dokumänt-Cloud-AI-Choschte.** OCR und Übersetzig laufe lokal; die vorwärtsgerichteti Zustandsmaschiine (`discovered → downloaded → ocr_done → translated → published`) garantiert, dass keis Dokumänt nomal verarbeitet wird, usser es hät sich g'änderet.
- **De Pipeline-Chärn hät keini Dritt-Abhängigkeite** — Parsing- / Manifest- / Delta-Modul laufe und teste uf eme saubere Python, ohni dass öpis mit pip-installiert isch; d'OCR/Übersetzigs-Stufe degradiered würdevoll, wenn optionali Paket fehled.
- **D'Edge-Site** wendet schträngi Sicherheits-Header + CSP a (keis `unsafe-inline`; inline JSON-LD isch sha256-pinned), Schpraach-Verhandlig via `Accept-Language` + Länderzuedeilig, en 30-tägige KV-Siite-Cache und en tägliche Huushaltigs-Cron.
- **Inkrementelli Updates:** en Delta-Detektor vergliicht de Quell-Index und speist nur Änderige zrugg i d'Pipeline.

### Für Entwickler

D'öffentischi API under https://www.ufolens.com/api/v1 git Dokumänt und Metadate als JSON zrugg. De anonym Zuegriff isch rate-limitiert; für Forscher-/Entwickler-Zuegriff chasch en Key aafordere. Lueg de API-Abschnitt uf de Siite für Endpunkt und Limits.

### Status

Code komplett; Siite deployed uf https://www.ufolens.com. D'Produktions-Datebank wird gfüllet, indem d'Offline-Pipeline usgfüehrt wird und s'Bundle vorwärts veröffentlicht wird (`cli_publish run --remote`). Di vollständige Design-Dokumänt sind in `docs/20260511/`.

### Lizänz / Gränze

- Quälledokumänt: Werke vo de US-Bundesregierig, Public Domain innerhalb vo de USA.
- De Code vo dere Plattform: lueg `LICENSE`.
- D'Siite schickt `Tdm-Reservation: 1` und `X-Robots-Tag: noai, noimageai` — indexierbar für Suechmaschiine, opted-out vo AI-Training/Scraping.
- Videomaterial wird DVIDS / AARO zuegschribe und wird nöd vo dem Projekt beansprucht.

Issues und PRs sind willkomme. Bitte lies `CLAUDE.md` und `docs/20260511/00-*`, bevor du strukturelli Änderige vorschlasch.
