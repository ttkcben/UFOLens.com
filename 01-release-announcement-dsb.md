# GitHub — Póst 1 z 3 · Oznajenje wózjawjenja / blok README

**Wužyś:** ako śěło GitHub Release, pśipěta diskusija abo górny źěl repo README.
**Klucowe słowa:** UAP, UFO, PURSUE archiw, zjawjone dokumenty, wótwórjone daty, połnotekstowe pytanje, OCR, mašinowe pśełožowanje, lokalny LLM, Ollama, edge computing, zjawny API, Hono, TypeScript, Python
**Hyperwótkaze:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — wěcejrěcna, pśepytujobna platforma za archiw PURSUE UAP

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Žrědłowy archiw:** https://www.war.gov/ufo

`ufolens.com` wózjawja znowego archiw **PURSUE** wójskego departamenta ZDA wó zjawjonych UAP / UFO-zapiskach ako platformu znanja: połnotekstowe pytanje, mašinowe pśełožowanje pśez ceły korpus, pśepytowanje na karśe a casowej liniji a zjawny JSON API. Žrědłowe dokumenty su źěła zwězkowego kněžaŕstwa ZDA a su w ZDA zjawnosći pśistupne ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Toś ten projekt **njejo zwězany z kněžaŕstwom ZDA**, njewužywa žedne oficielne insignije a nigda njewótwrośijo redakcije.

### Architektura

```
Lokalny computer (Apple Silicon, priwatny IP)        Kšomowa seś
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   boki
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   zjawny API
  translate / NER: local LLM (Gemma via Ollama)        /admin        konsola operatora
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Nulowe kószty za kクラウド-AI na dokument.** OCR a pśełožowanje se lokalnje wóterguju; forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) garantěrujo, až se žeden dokument znowego njepśeźěłowa, snaźkuli se jo změnił.
- **Jědro pipeline'a njama žedne dependentnosći tśeśich** — parsowanske / manifestowe / delta-moduly se wuwjeźu a testuju na cystym Python bźez nicego pip-instalěrowanego; OCR/pśełožowańske schójźenki se pśiměrje degraděruju, gaž opcionalne pakety feluju.
- **Bok na kšomje** nałožujo stroge wěstotne głowy + CSP (žeden `unsafe-inline`; inline JSON-LD sha256-pśipěty), rěcne dogronjenje pśez `Accept-Language` + krajowe mapowanje, 30-dnjowy KV-cache za boki a wšedny cron za porěźenje.
- **Inkrementalne aktualizacije:** delta-detektor pśirownajo žrědłowy indeks a zawrośi jano změny do pipeline'a.

### Za wuwijarjow

Zjawny API na https://www.ufolens.com/api/v1 wrośi dokumenty a metadaty ako JSON. Anonymny pśistup jo na licbje wobgranicowany; pominajśo se wó kluc za slěźaŕske/wuwijaŕske rowniny. Glědajśo API-wótrězk na boku za kóńcne dypki a limity.

### Status

Kod jo dokóńcony; bok jo na https://www.ufolens.com nasajźony. Produkcijska datowa banka se napołnjujo pśez wuwjeźenje offline-pipeline'a a wózjawjenje paketa (`cli_publish run --remote`). Połne designowe dokumenty su w `docs/20260511/`.

### Licenca / granicnosći

- Žrědłowe dokumenty: źěła zwězkowego kněžaŕstwa ZDA, w ZDA zjawnosći pśistupne.
- Swójski kod toś teje platformy: glědaj `LICENSE`.
- Bok pósćela `Tdm-Reservation: 1` a `X-Robots-Tag: noai, noimageai` — indeksěrujobny pśez pytawy, wótzjawjony z AI-treněrowanja/scrapinga.
- Videomaterial se pśipisujo DVIDS / AARO a se pśez toś ten projekt njenadpadnjo.

Problemy a PRs su witane. Pšosym pśecytajśo `CLAUDE.md` a `docs/20260511/00-*`, nježli až wótwóriśo strukturelne změny.

