# GitHub — Bidrag 1 vun 3 · Release / README-Ankünnigen-Block

**Bruken as:** en GitHub-Release-Text, en fastpinnte Diskusschoon oder an de Spitz vun de Repo-README.
**Slötelwöör:** UAP, UFO, PURSUE-Archiv, deklassifizeerte Dokumenten, apen Daten, Vulltext-Söök, OCR, maschinelle Översetten, lokaal LLM, Ollama, Edge Computing, apentliche API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — en mehrsprakige, dörsökbore Plattform för dat PURSUE UAP-Archiv

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Born-Archiv:** https://www.war.gov/ufo

`ufolens.com` publizeert dat **PURSUE**-Archiv vun’t U.S. War Department mit deklassifizeerte UAP / UFO-Opteken as en Wetenplattform: Vulltext-Söök, maschinelle Översetten över den helen Korpus, Utforschen mit Koort un Tiedlien un en apentliche JSON API. De Born-Dokumenten sünd Warken vun de U.S.-Bundsregeren un sünd binnen de USA Public Domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dit Projekt is **nich mit de U.S.-Regeren verbunnen**, bruukt keen offiziellen Insignien un maakt Redaktschonen nienich trüggängig.

### Architektur

```
Lokaal Reekner (Apple Silicon, private IP)           Edge-Nettwark
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only Karn)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (blots vörwärts)   /{lang}/...   Sieden
  OCR: Open-Source-Engine (Tesseract CLI Fallback)     /api/v1/...   apentliche API
  translate / NER: lokaal LLM (Gemma över Ollama)      /admin        Bedriever-Konsool
  Tostand: SQLite-Manifest                           stütt dör: Edge SQL DB, Objekt-
        │                                              spieker (Born-PDFs), KV-Cache
        └── publizeert en Bünnel: SQL + Asset-Manifest + Cache-Purge-List ──┘
```

- **Keen Kosten pro Dokument dör Cloud-AI.** OCR un Översetten loopt lokal; de blots-vörwärts-Tostandsmaschien (`opdeckt → rünnerladen → ocr_fardig → översett → publizeert`) garanteert, dat keen Dokument wedder bearbeidt warrt, wenn dat sik nich ännert hett.
- **De Karn vun de Pipeline hett keen Afhängigkeiten vun Drüddanbeders** — Parsing-, Manifest- un Delta-Modulen loopt un warrt test op en schier Python ahn wat mit pip installeert to hebben; OCR-/Översetten-Stopen warrt graziöös degradeert, wenn optionale Paketen fehlt.
- **De Edge-Siet** wendt strikte Sekerheits-Headers + CSP an (keen `unsafe-inline`; Inline-JSON-LD is mit sha256 fastpVolkswagen), Spraakverhanneln över `Accept-Language` + Länner-Mapping, en 30-Daag-KV-Sietencache un en daaglichen Housekeeping-Cron.
- **Inkrementelle Updates:** En Delta-Detektor vergliekt den Born-Index un speist blots Ännern trügg in de Pipeline.

### För Entwicklers

De apentliche API op https://www.ufolens.com/api/v1 gifft Dokumenten un Metadaten as JSON trügg. Anonyme Togang is in de Rate begrenzt; fraagt na en Slötel för Forscher-/Entwickler-Stopen. Kiekt in den API-Afsnitt op de Siet na Endpunkten un Limits.

### Status

Kood fardig; Siet is op https://www.ufolens.com insett. De Produktschonsdatenbank warrt füllt, indem de Offline-Pipeline utföhrt un dat Bünnel vörwärts publizeert warrt (`cli_publish run --remote`). Vullstännige Design-Dokmenten leevt in `docs/20260511/`.

### Lizenz / Grenzen

- Born-Dokumenten: Warken vun de U.S.-Bundsregeren, Public Domain binnen de USA.
- De egen Kood vun disse Plattform: kiek `LICENSE`.
- De Siet sennt `Tdm-Reservation: 1` un `X-Robots-Tag: noai, noimageai` — indexeerbor dör Söökmotoren, utsloten vun AI-Training/Scraping.
- Videomateriaal warrt DVIDS / AARO toschreven un warrt nich vun dit Projekt beansprucht.

Issues un PRs sünd willkamen. Leest `CLAUDE.md` un `docs/20260511/00-*` vördat Ji strukturelle Ännern apenmaakt.

