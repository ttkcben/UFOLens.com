# GitHub — Beitrag 1 vo 3 · Release / README-Ankündigungsblock

**Vawendung:** Ois GitHub Release-Text, a ogheftete Diskussion, oder am Onfong vom Repo-README.
**Schlisslwörter:** UAP, UFO, PURSUE archive, deklassifizierte Dokumente, offane Datn, Voitextsuach, OCR, maschinelle Ibasetzung, lokals LLM, Ollama, Edge Computing, effentliches API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — a mehrsprochige, suachbore Plattform fia s PURSUE UAP-Archiv

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Quellnarchiv:** https://www.war.gov/ufo

`ufolens.com` vaeffentlicht s **PURSUE**-Archiv vom U.S. War Department mit deklassifizierten UAP / UFO-Akte ois Wissensplattform: Voitextsuach, maschinelle Ibasetzung iban gsamtn Korpus, Kartn + Zeitstrahl-Erkundung und a effentlichs JSON API. De Quellndokumente san Werke vo da US-Bundesregiarung und san in de USA gemeinfrei ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Des Projekt is **ned mit da US-Regiarung vabundn**, vawendt koane offizielln Obzeichn und mocht nia Schwärzunga rückgängig.

### Architektur

```
Lokala Rechna (Apple Silicon, private IP)        Edge-Netzwerk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   Seitn
  OCR: Open-Source-Engine (Tesseract CLI Fallback)     /api/v1/...   effentlichs API
  translate / NER: lokals LLM (Gemma via Ollama)       /admin        Betreiakonsole
  state: SQLite-Manifest                              unterstützt durch: Edge-SQL-DB, Objekt-
        │                                              speicha (Quell-PDFs), KV-Cache
        └── vaeffentlicht a Bündl: SQL + Asset-Manifest + Cache-Löschlistn ──┘
```

- **Nui Cloud-KI-Kostn pro Dokument.** OCR und Ibasetzung laffn lokal; de "forward-only"-Zustandsmaschin (`discovered → downloaded → ocr_done → translated → published`) garantiert, dass koa Dokument zwoamoi vaoarbeit werd, außer es hod si g'ändert.
- **Da Pipeline-Kern hot koane Drittanbieter-Abhängigkeitn** — Parsing- / Manifest- / Delta-Module laffn und testn auf am saubarn Python ohne pip-installierte Pakete; de OCR/Ibasetzungs-Stufn funktionian a, wenn optionale Pakete fehln, hoid mit Einschränkunga.
- **De Edge-Seitn** vawendt strikte Sicherheits-Header + CSP (koa `unsafe-inline`; inline JSON-LD is mit sha256-pinnt), Sprochvahandlung iba `Accept-Language` + Ländazuordnung, an 30-Tog-KV-Seitn-Cache und an täglichn Wartungs-Cronjob.
- **Inkrementelle Updates:** a Delta-Detektor vagleicht den Quellindex und gibt nua de Änderunga zruck in de Pipeline.

### Fia Entwickla

S'effentliche API unta https://www.ufolens.com/api/v1 liefert Dokumente und Metadatn ois JSON. Da anonyme Zuagriff is ratnlimitiert; frog an Schlissl o fia Forscha-/Entwickla-Stufn. Schaug da den API-Abschnitt auf da Seitn o fia Endpunkte und Limits.

### Status

Code is fertig; de Seitn is unta https://www.ufolens.com live. De Produktionsdatnbank werd befüllt, indem de Offline-Pipeline lafft und s'Bündl noch vorn vaeffentlicht werd (`cli_publish run --remote`). De voiständign Design-Doks findst in `docs/20260511/`.

### Lizenz / Grenzn

- Quellndokumente: Werke vo da US-Bundesregiarung, in de USA gemeinfrei.
- Da Code vo dera Plattform: schaug `LICENSE`.
- De Seitn sendt `Tdm-Reservation: 1` und `X-Robots-Tag: noai, noimageai` — duach Suachmaschinan indexierbor, oba vom KI-Training/Scraping ausgnumma.
- Video-Material werd DVIDS / AARO zuagschriebn und werd vo dem Projekt ned beansprucht.

Issues und PRs san willkomma. Bittschee `CLAUDE.md` und `docs/20260511/00-*` lesn, bevorst strukturelle Änderunga vorschlogst.

