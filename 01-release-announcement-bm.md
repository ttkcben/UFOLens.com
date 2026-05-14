# GitHub — Post 1 sur 3 · Kunnafoni / README kunnafoni blok

**A baara kɛ i n’a fɔ:** GitHub Kunnafoni jɛlen, Walima pinned Discussion, walima repo README sanfɛla.
**Daɲɛw:** UAP, UFO, PURSUE archive, sɛbɛnniw minnu bɔra gundo la, data dafalen, ɲinini kɛnɛ, OCR, masin-na-kalanko, LLM lokal, Ollama, edge computing, API jama bɛɛ ye, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP kɔnɔkow ka kanw caman ni ɲinini platfɔɔmu

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Sɛbɛnni fɔlɔw:** https://www.war.gov/ufo

`ufolens.com` bɛ Ameriki ka Kɛlɛ Departeman ka **PURSUE** kɔnɔkow minnu bɛ tali kɛ UAP / UFO sɛbɛnniw la, olu lajɛ kɛ ɲinini platfɔɔmu ye: ɲinini kɛnɛ, masin-na-kalanko kɔnɔna bɛɛ la, karti + waati laɲini, ani JSON API jama bɛɛ ye. Sɛbɛnni fɔlɔw ye Ameriki fanga ka baara ye, wa u bɛ jama bɛɛ bolo Ameriki kɔnɔ ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Nin poroje **tɛ tali kɛ Ameriki fanga la**, a tɛ fanga ka taama-shiya baara, wa a tɛ gundo bɔ abada.

### Dilancogo

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

- **Sɛbɛn o sɛbɛn tɛ AI wari sara bululu kɔnɔ.** OCR ni kalanko bɛ kɛ yɔrɔ-yɔrɔ la; state machine min bɛ taa ɲɛ (discovered → downloaded → ocr_done → translated → published) bɛ a garanti ko sɛbɛn si tɛ kɛ kokura fo ni a yɛlɛmana.
- **Pipeline core tɛ ni mɔgɔ sada ka dɛmɛni si ye** — parsing / manifest / delta modules bɛ baara ani u bɛ test kɛ Python gelen kan, pip-installed fɛn si tɔni; OCR/kalanko bɔnsɔnw bɛ wajibi-fɛnw bɔ ka nɔgɔya.
- **Edge site** bɛ sigida ni CSP (Content Security Policy) (tɛ `unsafe-inline`; inline JSON-LD sha256-pinned), kanw ɲɔgɔn sɔrɔ `Accept-Language` + jamana-kanw fɛ, KV page cache tile 30, ani cron min bɛ kɛ tile o tile.
- **Yɛlɛma kura:** delta detector bɛ fɔlɔ ka index fɔlɔw fara ɲɔgɔn kan, wa a bɛ yɛlɛma kura dɔrɔn de fara pipeline kan.

### Développeurw ye

API jama bɛɛ ye https://www.ufolens.com/api/v1 na, o bɛ sɛbɛnniw ni metadataw di JSON fɔlɔ. Mɔgɔ minnu tɛ dɔn, olu tɛ se ka nafa sɔrɔ kosɛbɛ; ɲinini kɛ ɲininiw la ɲɛnajɛla/dɛvelopɛriw ka taa. API bɔnsɔn lajɛ site kan ka endpointiw ni a danfɛnw ye.

### Hali

Code dafalen; site deployé la https://www.ufolens.com. Production database bɛ kɛ ka offline pipeline baara ani ka bundle lajɛ (`cli_publish run --remote`). Design docs dafalen bɛ `docs/20260511/` kɔnɔ.

### Lisansi / Danfɛnw

- Sɛbɛnni fɔlɔw: Ameriki fanga ka baara, jama bɛɛ bolo Ameriki kɔnɔ.
- Nin platfɔɔmu ka code yɛrɛ: `LICENSE` lajɛ.
- Site bɛ `Tdm-Reservation: 1` ani `X-Robots-Tag: noai, noimageai` ci — ɲinini motɛriw bɛ se k’a index, nka a bɔra AI laɲini/scraping la.
- Vidéo-sɛbɛnniw bɛɛ ye DVIDS / AARO ta ye, wa nin poroje ma fɔ u kan.

Geleya ni PRw bɛ jaabi. CLAUDE.md ni `docs/20260511/00-*` kalan sanni yɛlɛma cogo fɔlɔw dabɔ.

