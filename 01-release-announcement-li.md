# GitHub — Pos 1 vaan 3 · Aankondiging vaan de release / README

**Te gebruke es:** de teks vaan 'n GitHub Release, 'n vastgepinde Discussie, of bovenaon de README vaan 't repo.
**Trefwäörd:** UAP, UFO, PURSUE-archief, vrijgegeve documente, open data, full-text zeuke, OCR, automatische vertaoling, lokale LLM, Ollama, edge computing, publieke API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — e meertalig, doorzeukbaar platform veur 't PURSUE UAP-archief

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Bronarchief:** https://www.war.gov/ufo

`ufolens.com` herbubliceert 't **PURSUE**-archief vaan 't Amerikaans Oorlogsdepartement mèt vrijgegeve UAP / UFO-dossiers es e kinnesplatform: full-text zeuke, automatische vertaoling door 't ganse corpus, verkinning via kaart en tiedlijn, en 'n publieke JSON API. De brondocumente zien wèrke vaan de Amerikaanse federale euverheid en zien in de VS publiek domein ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dit projek is **neet geaffilieerd mèt de Amerikaanse euverheid**, gebruuk gein officieel logo's en maak noets redacties oongedoon.

### Architectuur

```
Lokaal masjien (Apple Silicon, residentieel IP)      Edge-netwerk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only kern)           worker/  (TypeScript, Hono.js)
  ophale → OCR → vertaole → publicere (allein-nao-veure)    /{lang}/...   pagina's
  OCR: open-source-engine (Tesseract CLI-fallback)     /api/v1/...   publieke API
  vertaole / NER: lokale LLM (Gemma via Ollama)        /admin        console veur operator
  staot: SQLite-manifes                             oondersteund door: edge SQL DB, object-
        │                                              opslag (bron-PDF's), KV-cache
        └── publiceert 'ne bundel: SQL + asset-manifes + cache-opsjoonlies ──┘
```

- **Nul koste per document veur cloud-AI.** OCR en vertaoling weure lokaal oetgeveurd; de allein-nao-veure-staotsmachine (`ontdek → gedownload → ocr_klaor → vertaald → gepubliceerd`) garandeert tot gei document obbenuits weurt verwerk, tenzij 't is veranderd.
- **De kern vaan de pipeline heet gein aofhenkelekhede vaan daarde** — de parsing-, manifes- en delta-modules drejje en teste op 'ne kale Python boe niks is geïnstalleerd mèt pip; de OCR/vertaolingsfases valle sierlek trök wienie optioneel pakkette oontbreke.
- **De edge-site pas strikte beveiligingsheaders + CSP toe** (gein `unsafe-inline`; inline JSON-LD is mèt sha256-gepind), taoloonderhandeling via `Accept-Language` + landkaarting, 'ne 30-daagse KV-pagina-cache, en 'ne daaglikse oonderhoudscron.
- **Incrementeel updates:** 'nen delta-detector vergeliek de brondindex en veurt allein veranderinge trök in de pipeline.

### Veur oontwikkelere

De publieke API op https://www.ufolens.com/api/v1 geuf documente en metadata trök es JSON. Anonieme touwgaank is beperk in snelheid; vraog 'ne sleutel aon veur oonderzeuker-/oontwikkelaarsniveaus. Zuug de API-sectie op de site veur endpoints en limiete.

### Status

Code compleet; site ingezat op https://www.ufolens.com. De productiedatabase weurt gevöld door de offline pipeline te drejje en de bundel nao veure te publicere (`cli_publish run --remote`). Volledige oontwerpdoc's stoon in `docs/20260511/`.

### Licentie / grenze

- Brondocumente: wèrke vaan de Amerikaanse federale euverheid, publiek domein in de VS.
- De eige code vaan dit platform: zuug `LICENSE`.
- De site sjik `Tdm-Reservation: 1` en `X-Robots-Tag: noai, noimageai` — indexeerbaar door zeukmesjiene, aofgemeld veur AI-training/scraping.
- Videobeelde weure touwgesjreve aon DVIDS / AARO en weure neet door dit projek geclaimd.

Issues en PRs zien wèlkome. Lees iers `CLAUDE.md` en `docs/20260511/00-*` veurtot geer structureel veranderinge veurstèlt.

