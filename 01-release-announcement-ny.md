# GitHub — Lemba 1 mwa 3 · Gawo lofotokoza za Release / README

**Gwiritsani ntchito ngati:** thupi la GitHub Release, Discussion yomwe yakhazikika (pinned), kapena pamwamba pa README ya repo.
**Mawu oyamba (Keywords):** UAP, UFO, archive ya PURSUE, zikalata zomwe zatulutsidwa (declassified documents), data yotseguka, kufufuza mawu onse (full-text search), OCR, kumasulira kwa makina, local LLM, Ollama, edge computing, API ya pampalame, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — pulatifomu yomwe ili ndi zinenero zingapo ndipo ikhoza kufufuzidwa ya archive ya PURSUE UAP

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Source archive:** https://www.war.gov/ufo

`ufolens.com` ikuperekanso archive ya **PURSUE** ya U.S. War Department ya zikalata za UAP / UFO zomwe zatulutsidwa ngati pulatifomu ya chidziwitso: kufufuza mawu onse, kumasulira kwa makina m'zinenero zosiyanasiyana, kufufuza kudzera m'mapu + timeline, komanso JSON API ya pampalame. Zikalata zoyambira ndi ntchito za boma la federal la U.S. ndipo mkati mwa U.S. ndi za public domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Projekitiyi **si ya boma la U.S.**, sikutumiza zizindikiro zof 공식 (official insignia), ndipo siphunzitsanso zomwe zafukidwa (redactions).

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

- **Mtengo wa 0 wa cloud-AI pa chikalata chilichonse.** OCR ndi kumasulira kumachitikira m'makina a m'deralo (locally); state machine yoyenda kutsogolo (`discovered → downloaded → ocr_done → translated → published`) imatsimikizira kuti palibe chikalata chomwe chidzaperekedwanso pokhapokha chikasinthidwa.
- **Pipeline core ilibe third-party dependencies** — parsing / manifest / delta modules imagwiritsidwa ntchito ndikuyesedwa pa Python yatsopano popanda chilichonse chomwe chaikidwa kudzera m'pip; gawo la OCR/translation limapitiriza kugwira ntchito ngakhale pakasoŵa mapakala osankha (optional packages).
- **Edge site** imagwiritsitsa security headers zowawa + CSP (palibe `unsafe-inline`; inline JSON-LD sha256-pinned), kusamalira zinenero kudzera m' `Accept-Language` + country mapping, KV page cache ya masiku 30, komanso daily housekeeping cron.
- **Incremental updates:** delta detector imafufuza kusiyana kwa source index ndikupereka zosintha zokha mkati mwa pipeline.

### For developers

API ya pampalame pa https://www.ufolens.com/api/v1 imabwera ndi zikalata ndi metadata ngati JSON. Kupeza zinthu popanda dzina (anonymous access) kuli ndi malire; pemphani key ya m’magulu a researcher/developer. Onani gawo la API pa siteyo kuti muone endpoints ndi malire.

### Status

Code ili zokwanira; site yakhazikitsidwa pa https://www.ufolens.com. Production database imadzaza populumitsa offline pipeline ndikupereka bundle kutsogolo (`cli_publish run --remote`). Zikalata zonse za design zili mu `docs/20260511/`.

### License / boundaries

- Zikalata zoyambira: ntchito za boma la federal la U.S., public domain mkati mwa U.S.
- Code ya pulatifomu iyi: onani `LICENSE`.
- Siteyi imatumiza `Tdm-Reservation: 1` ndi `X-Robots-Tag: noai, noimageai` — zomwe zitha kufufuzidwa ndi search engines, koma yasala kuphunzitsa AI/scraping.
- Mavideo ndi a DVIDS / AARO ndipo projekitiyi sikutonthoza kuti ndi ake.

Issues ndi PRs zilandiridwa. Chonde werengani `CLAUDE.md` ndi `docs/20260511/00-*` musanayambe kusintha zoyikika (structural changes).