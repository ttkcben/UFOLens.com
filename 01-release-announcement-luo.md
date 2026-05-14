# GitHub — Otamo 1 mar 3 · Goyo / otamo mar somo (README)

**Ti gi kaka:** golo mar GitHub, Twak molos kata kaka sirkal mar README.
**Weche madongo:** UAP, UFO, archive mar PURSUE, gasede mopondo, kamba mar data, manyo kuonde duto, OCR, lokruok mar dho-kompyuta, LLM man piny, Ollama, tiyo gi kompyuta man e bath, API mar ji duto, Hono, TypeScript, Python
**Kuonde manyo-weche:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — kamba man gi dhok mang'eny, ma inyalo manyoe weche e archive mar PURSUE UAP

**Koro otugo:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archive ma oa:** https://www.war.gov/ufo

`ufolens.com` ogoyo kendo archive mar U.S. War Department mar **PURSUE** mar gasede mopondo mag UAP / UFO kaka kamba mar gonyo ng'eyo: manyo weche duto, lokruok mar dho-kompyuta e gasede duto, rameny + manyo kaka gasede nindo e odiechieng' gi nyaka, kod API mar JSON mar ji duto. Gasede ma oa e sirkal mar U.S. ma e piny U.S. gin mag ji duto ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Tiend loch mar achiel e kamba mar ji duto **ok ochuok gi sirkal mar U.S.**, ok otii gi alama mar sirkal, kendo ok oloch weche mopondo.

### Lokruok mar gonyo

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

- **Sifuri mar tiyo gi AI mar kamba.** OCR kod lokruok otii gi kama piny; tiyo gi gonyo kaka `discovered → downloaded → ocr_done → translated → published` okonyo mondo gasede duto ok oti gi nyaka golo, kendo gasede duto ok otii gi nyaka golo weche machol.
- **Tiend loch mar achiel mar kamba ok owinjo mondo oti gi kamba mar ji duto** — parsing / manifest / delta modules otii kendo oti gi Python moti gi nyaka golo, kendo OCR/translation stages ok owinjo mondo oti gi kamba mar ji duto.
- **Kamba mar ji duto** otii gi headers mar manyo mar ji duto + CSP (no `unsafe-inline`; inline JSON-LD sha256-pinned), manyo dhok gi `Accept-Language` + country mapping, KV page cache mar dieng' 30, kendo cron mar londo mar ji duto.
- **Lokruok mar ji duto:** manyo weche makaka okonyo manyo weche makaka okonyo manyo weche makaka okonyo manyo weche makaka okonyo manyo weche makaka okonyo manyo weche makaka okonyo manyo weche makaka ok=== FILE 01-release-announcement-luo.md ===
# GitHub — Gikwanyisi mar 1 kuom 3 · Twak Mar Nyiswa / Twak mar README

**Ti kaka:** Kaka weche mag GitHub Release, Twak ma ochung' motegno, kata achiel kuom weche manie README mar repo.
**Weche Mokonyo:** UAP, UFO, PURSUE archive, gik mang'ado siri, data ma oyawo, manyo gik duto manie iye, OCR, lendo gi masin, LLM mar gweng', Ollama, edge computing, API mar ji duto, Hono, TypeScript, Python
**Wanjruok mag Kony:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — pulatifom ma nigi dhok mang'eny, ma inyalo manyo godo gik mang'ado siri mag PURSUE UAP

**Mangima:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Manyo archive:** https://www.war.gov/ufo

`ufolens.com` lendo kendo gik mang'ado siri mag U.S. War Department mar **PURSUE** UAP/UFO kaka pulatifom mar ng'eyo gik: manyo gik duto manie iye, lendo gi masin e gwenge duto, manyo e ramani + ranyisi mar seche, kendo API mar JSON ma okonjo ji. Gik mang'ado siri mag ji duto gin mag sirkal mar U.S. kendo manie gweng' mar U.S. gin mag ji duto ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Puojectni **ok oton'g' gi sirkal mar U.S.**, ok oti gi ranyisi moro amora mar sirkal, kendo ok osemanyore gi gik ma ne okanore.

### Chokruok

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

- **Ne ochor godo AI mar gweng'.** OCR kod lendo timore e gweng'; gi `discovered → downloaded → ocr_done → translated → published` gin ma ok omachore, ok onyal lokore apaa.
- **Pipeline core ok oton'g' gi gik manyien.** Manyo gik duto manie iye / manyo gik duto / delta modules timore e Python maler ma onge gik manyien ma ne oyieng'ore, OCR/translation stages giyore achama achama ka gik manyien ok one.
- **Edge site** nigi headers kod CSP (no `unsafe-inline`; inline JSON-LD sha256-pinned), wacho dhok kaka `Accept-Language` + manyo gweng', KV page cache mar chieng' 30, kod cron mar puodho gik duto.
- **Konyruok ma dhi nyime:** delta detector nigi kanyachiel, kendo gik ma osemanyore duto nyalo lokore.

### Ne gitich

API mar ji duto e https://www.ufolens.com/api/v1 nyiso gik mang'ado siri kod metadata kaka JSON. Ji duto nyalo manyo ma ok ogeng'ore; manyo nyako mar gik manyien e gweng' mar researcher/developer. Ne kinde mag API manie kanyachiel mag manyo gik duto.

### Status

Code osekonyore; site osemieng' e https://www.ufolens.com. Database mar ji duto osemieng' gi pipeline ma ok one gi wang' kendo gik manyien duto nyalo lokore (`cli_publish run --remote`). Gik mang'ado siri manie `docs/20260511/`.

### License / gik duto

- Manyo gik duto: gik mang'ado siri mag sirkal mar U.S., mag ji duto manie U.S.
- Code mar pulatifomni: ne `LICENSE`.
- Siteni oromo `Tdm-Reservation: 1` kod `X-Robots-Tag: noai, noimageai` — inyalo manyo gi manyo gik duto, ok oyieng'ore gi AI training/scraping.
- Vidio ochieng'ore gi DVIDS / AARO kendo ok oton'g' gi puojectni.

Issues kod PRs oyie. Yiere `CLAUDE.md` kod `docs/20260511/00-*` ka pok ojawuok kendo manyore gi gik manyien.

