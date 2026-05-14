# GitHub — Icipande 1 pali 3 · Ukulekula / Ukubilisha mu README

**Cibomfiwe nga:** Umubili wa GitHub Release, Ifyalandwe fyatekelwe, nelyo pa Mutwe wa README wa repo.
**Amashiwi akulu:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — icikuulwa cikalamba, icisangako ifyakufwaya, icabela indimi ishingi ku bubungwe bwa PURSUE UAP

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Ububungwe bwa kulekanina:** https://www.war.gov/ufo

`ufolens.com` ikubilisha nakabili ububungwe bwa U.S. War Department bwa **PURSUE** bwa mafunde ya UAP / UFO ayashibishwe ku bantu nge cikuulwa ca kwishiba: ukufwaya ifyabu fyonse, ukupilibula kwa muchini pa fyonse, ukwafwaya pa mapu + timeline, ne public JSON API. Amafunde ya kulekanina yaba imilimo ya U.S. federal government kabili mu U.S. yali public domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Uyu mulimo **tawaba ne filubo ne U.S. government**, tabomfya isonko lyakubilisha, kabili takapilibule ifyashibishwe.

### Architecture

```
Local machine (Apple Silicon, residential IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Pantu takwaba indalama sha cloud-AI pa document iyonse.** OCR na ukupilibula fipita mu cifulo; ifya kubomfya fya forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) filanga ukuti takwaba document iyapilibulwa nakabili kano yalikwetepo ifyacinchika.
- **Pipeline core tayakwata third-party dependencies** — parsing / manifest / delta modules fipita kabili fipimwa pa Python isuma ishakwata ifyacilamo; OCR/translation stages fikalala bwino nga takwaba optional packages.
- **Edge site** ibomfya strict security headers + CSP (takwaba `unsafe-inline`; inline JSON-LD sha256-pinned), ukukonka ululimi ukubomfya `Accept-Language` + ukufwanisha kwa calo, 30-day KV page cache, na daily housekeeping cron.
- **Incremental updates:** delta detector ilatila ifilengwa fya source index kabili ilapeelelela ifyacinchika mu pipeline.

### Ku bantu babombele pa mafunde

Public API pa https://www.ufolens.com/api/v1 ibwesesha amafunde na metadata nga JSON. Ukubomfya kwa anonymous kuli na rate-limited; fwailapo key ku bantu bafwaya ukufwaya ifyacindama / babombele pa mafunde. Moneni API section pa site pa mafumu na limits.

### Status

Code yapwa; site yatwikwa pa https://www.ufolens.com. Production database yakwata ifyacindama mu kubomfya offline pipeline na ukubilisha bundle forward (`cli_publish run --remote`). Full design docs shasangwa mu `docs/20260511/`.

### License / boundaries

- Amafunde ya kulekanina: Imilimo ya U.S. federal government, public domain mu U.S.
- Code ya uyu mulimo: moneni `LICENSE`.
- Site ituma `Tdm-Reservation: 1` na `X-Robots-Tag: noai, noimageai` — ikasangwako ku search engines, tayakabomfiwe ku AI training/scraping.
- Video footage yatumbikwa ku DVIDS / AARO kabili tayalilwa ku uyu mulimo.

Ifipusho na PRs welcome. Palenjeni ukubelenga `CLAUDE.md` na `docs/20260511/00-*` libela ukushilula ifyacinchika fya mu kuilenga.

