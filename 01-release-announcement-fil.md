# GitHub — Post 1 ng 3 · Anunsyo ng Release / README

**Gamitin bilang:** isang katawan ng GitHub Release, isang naka-pin na Discussion, o sa itaas ng repo README.
**Keywords:** UAP, UFO, PURSUE archive, mga declassified na dokumento, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — isang multilinggwal at nahahanap na platform para sa PURSUE UAP archive

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Source archive:** https://www.war.gov/ufo

Ang `ufolens.com` ay muling inilalathala ang **PURSUE** archive ng U.S. War Department ng mga declassified na UAP / UFO records bilang isang knowledge platform: full-text search, machine translation sa buong corpus, pag-explore sa mapa + timeline, at isang pampublikong JSON API. Ang mga source document ay mga gawa ng pederal na pamahalaan ng U.S. at sa loob ng U.S. ay nasa pampublikong domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ang proyektong ito ay **hindi kaakibat sa gobyerno ng U.S.**, hindi gumagamit ng opisyal na sagisag, at kailanman ay hindi binabaligtad ang mga redaction.

### Arkitektura

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

- **Zero per-document cloud-AI cost.** Ang OCR at translation ay tumatakbo nang lokal; tinitiyak ng forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) na walang dokumentong muling ipoproseso maliban kung ito ay nagbago.
- **Ang pipeline core ay walang third-party dependencies** — ang parsing / manifest / delta modules ay tumatakbo at sumusubok sa isang malinis na Python na walang anumang naka-pip-install; ang mga yugto ng OCR/translation ay maganda ang pagbaba ng kalidad kapag wala ang mga opsyonal na package.
- **Ang Edge site** ay naglalapat ng mahigpit na security headers + CSP (walang `unsafe-inline`; ang inline na JSON-LD ay sha256-pinned), negosasyon ng wika sa pamamagitan ng `Accept-Language` + pagmamapa ng bansa, isang 30-araw na KV page cache, at isang araw-araw na housekeeping cron.
- **Mga Incremental update:** isang delta detector ang nag-iiba sa source index at nagpapakain lamang ng mga pagbabago pabalik sa pipeline.

### Para sa mga developer

Ang pampublikong API sa https://www.ufolens.com/api/v1 ay nagbabalik ng mga dokumento at metadata bilang JSON. Ang anonymous na access ay may rate-limit; humiling ng isang key para sa mga antas ng researcher/developer. Tingnan ang seksyon ng API sa site para sa mga endpoint at limitasyon.

### Katayuan

Kumpleto na ang code; naka-deploy ang site sa https://www.ufolens.com. Ang production database ay pinupunan sa pamamagitan ng pagpapatakbo ng offline na pipeline at pag-publish ng bundle pasulong (`cli_publish run --remote`). Ang buong mga dokumento ng disenyo ay nasa `docs/20260511/`.

### Lisensya / mga hangganan

- Mga source document: Mga gawa ng pederal na pamahalaan ng U.S., pampublikong domain sa loob ng U.S.
- Sariling code ng platform na ito: tingnan ang `LICENSE`.
- Nagpapadala ang site ng `Tdm-Reservation: 1` at `X-Robots-Tag: noai, noimageai` — maaaring i-index ng mga search engine, nag-opt out sa AI training/scraping.
- Ang video footage ay iniugnay sa DVIDS / AARO at hindi inaangkin ng proyektong ito.

Malugod na tinatanggap ang mga isyu at PR. Mangyaring basahin ang `CLAUDE.md` at `docs/20260511/00-*` bago magbukas ng mga pagbabago sa istruktura.

