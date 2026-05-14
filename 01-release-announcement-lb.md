# GitHub — Bäitrag 1 vun 3 · Release / README Ukënnegungsblock

**Benotzen als:** en GitHub Release Text, eng ugepinnte Diskussioun, oder uewen am README vum Repo.
**Schlësselwierder:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — eng méisproocheg, duerchsichbar Plattform fir d'PURSUE UAP-Archiv

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Quellarchiv:** https://www.war.gov/ufo

`ufolens.com` verëffentlecht d'**PURSUE**-Archiv vum U.S. War Department mat deklasséierten UAP / UFO-Opzeechnungen als Wëssensplattform nei: Volltext-Sich, maschinell Iwwersetzung iwwert de ganze Corpus, Kaarten- an Zäitlinn-Exploratioun, an eng ëffentlech JSON API. Quell-Dokumenter si Wierker vun der US-Bundesregierung an an den USA Public Domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dëse Projet ass **net mat der US-Regierung affiliéiert**, benotzt keng offiziell Insignien a mécht ni Redaktiounen réckgängeg.

### Architektur

```
Lokal Maschinn (Apple Silicon, Privat-IP)      Edge-Netzwierk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only Kär)          worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (nëmme virun)   /{lang}/...   Säiten
  OCR: Open-Source-Engine (Tesseract CLI Fallback)     /api/v1/...   ëffentlech API
  translate / NER: lokal LLM (Gemma via Ollama)        /admin        Bedreiwer-Konsol
  Zoustand: SQLite Manifest                            ënnerstëtzt vun: Edge SQL DB, Object
        │                                              Storage (Quell-PDFen), KV Cache
        └── verëffentlecht e Bündel: SQL + Asset-Manifest + Cache-Purge-Lëscht ──┘
```

- **Null Cloud-AI-Käschte pro Dokument.** OCR an Iwwersetzung lafe lokal; d'Forward-only State Machine (`discovered → downloaded → ocr_done → translated → published`) garantéiert, datt keen Dokument nei veraarbecht gëtt, ausser et huet geännert.
- **De Kär vun der Pipeline huet keng Drëtt-Ubidder-Ofhängegkeeten** — Parsing- / Manifest- / Delta-Module lafen a ginn op engem proppere Python ouni pip-installéiert Päck getest; OCR-/Iwwersetzungs-Etappe funktionéieren och ouni optional Päck, wann dës feelen.
- **Den Edge-Site** applizéiert streng Sécherheets-Headeren + CSP (keng `unsafe-inline`; inline JSON-LD ass mat sha256 ugepinn), Sprooch-Verhandlung iwwer `Accept-Language` + Land-Mapping, en 30-Deeg KV Säite-Cache, an en deeglechen Housekeeping-Cron.
- **Inkrementell Aktualiséierungen:** en Delta-Detekter vergläicht de Quell-Index an fiddert nëmmen Ännerungen zréck an d'Pipeline.

### Fir Entwéckler

D'ëffentlech API op https://www.ufolens.com/api/v1 gëtt Dokumenter a Metadaten als JSON zréck. Anonymen Zougank ass limitéiert; frot e Schlëssel fir Fuerscher-/Entwéckler-Tier un. Kuckt d'API-Sektioun op der Websäit fir Endpunkten a Limitten.

### Status

Code fäerdeg; Site live op https://www.ufolens.com. D'Produktiouns-Datebank gëtt populéiert andeems d'Offline-Pipeline ausgefouert gëtt an de Bündel no vir publizéiert gëtt (`cli_publish run --remote`). Déi komplett Design-Dokumenter sinn an `docs/20260511/`.

### Lizenz / Grenzen

- Quell-Dokumenter: Wierker vun der US-Bundesregierung, Public Domain an den USA.
- De Code vun dëser Plattform: kuckt `LICENSE`.
- De Site schéckt `Tdm-Reservation: 1` an `X-Robots-Tag: noai, noimageai` — indexéierbar vu Sichmaschinnen, ofgemellt vum AI-Training/Scraping.
- Video-Material gëtt DVIDS / AARO zougeschriwwen a gëtt net vun dësem Projet beusprocht.

Issues a PRs si wëllkomm. Liest w.e.g. `CLAUDE.md` an `docs/20260511/00-*` ier Dir strukturell Ännerungen virschloe kënnt.
