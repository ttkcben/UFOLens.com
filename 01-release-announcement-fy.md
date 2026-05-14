# GitHub — Post 1 fan 3 · Frijlitting / README-oankundigingsblok

**Brûk as:** in GitHub Release-body, in fêstmakke Diskusje, of boppe-oan de repo README.
**Kaaiwurden:** UAP, UFO, PURSUE-argyf, frijjûne dokuminten, iepen data, folsleine-tekst sykje, OCR, masine-oersetting, lokale LLM, Ollama, edge computing, publike API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — in meartalich, sykjeber platfoarm foar it PURSUE UAP-argyf

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Boarne-argyf:** https://www.war.gov/ufo

`ufolens.com` publisearret it **PURSUE**-argyf fan frijjûne UAP / UFO-records fan it Amerikaanske Oarlochsdepartemint op 'e nij as in kennisplatfoarm: folsleine-tekst sykje, masine-oersetting oer it hiele korpus, ferkenning fan kaart + tiidline, en in publike JSON API. Boarnedokuminten binne wurken fan de Amerikaanske federale oerheid en binne binnen de F.S. publyk domein ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dit projekt is **net ferbûn mei de Amerikaanske oerheid**, brûkt gjin offisjele insignia, en makket nea redaksjes ûngedien.

### Arsjitektuer

```
Lokale masine (Apple Silicon, partikulier IP)         Edge-netwurk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only kearn)          worker/  (TypeScript, Hono.js)
  helje → OCR → oersette → publisearje (allinich foarút)    /{lang}/...   siden
  OCR: iepenboarne-motor (Tesseract CLI fallback)     /api/v1/...   publike API
  oersette / NER: lokale LLM (Gemma fia Ollama)       /admin        operator-konsole
  steat: SQLite-manifest                            stipe troch: edge SQL DB, objekt-
        │                                             opslach (boarne-PDF's), KV-cache
        └── publisearret in bondel: SQL + asset-manifest + cache-leegje-list ──┘
```

- **Nul kosten foar wolk-AI per dokumint.** OCR en oersetting rinne lokaal; de allinich-foarút state machine (`ûntdutsen → ynladen → ocr_klear → oerset → publisearre`) garandearret dat gjin dokumint op 'e nij ferwurke wurdt, útsein as it feroare is.
- **De kearn fan de pipeline hat gjin ôfhinklikens fan tredden** — parse- / manifest- / delta-modules rinne en teste op in skjinne Python sûnder wat pip-ynstallearre; OCR/oersetting-stadia falle sierlik werom as opsjonele pakketten ûntbrekke.
- **Edge-side** past strange feiligens-headers + CSP ta (gjin `unsafe-inline`; inline JSON-LD sha256-fêstmakke), taalûnderhanneling fia `Accept-Language` + lân-mapping, in 30-dagen KV-side-cache, en in deistige ûnderhâlds-cron.
- **Stapsgewize updates:** in delta-detektor fergeliket de boarne-yndeks en fiedt allinich feroarings werom yn de pipeline.

### Foar ûntwikkelders

De publike API op https://www.ufolens.com/api/v1 jout dokuminten en metadata as JSON werom. Anonyme tagong hat in taryflimyt; freegje in kaai oan foar ûndersikers-/ûntwikkeldersnivo's. Sjoch de API-seksje op de side foar einpunten en limyten.

### Status

Koade kompleet; side ynset op https://www.ufolens.com. De produksjedatabase wurdt befolke troch de offline pipeline te rinnen en de bondel foarút te publisearjen (`cli_publish run --remote`). Folsleine ûntwerp-dokuminten steane yn `docs/20260511/`.

### Lisinsje / grinzen

- Boarnedokuminten: wurken fan de Amerikaanske federale oerheid, publyk domein binnen de F.S.
- De eigen koade fan dit platfoarm: sjoch `LICENSE`.
- De side stjoert `Tdm-Reservation: 1` en `X-Robots-Tag: noai, noimageai` — yndeksearber troch sykmasines, útskeakele foar AI-training/scraping.
- Fideobylden wurde taskreaun oan DVIDS / AARO en wurde net opeaske troch dit projekt.

Issues en PRs binne wolkom. Lês asjebleaft `CLAUDE.md` en `docs/20260511/00-*` foardat jo strukturele wizigings iepenje.
