# GitHub — Post 1 of 3 · Haroro / README tokana sivarai palaka

**Bamona be:** GitHub Haroro tamona, Diskausen ta e haginia, o repo README ena ataiai.
**Chea herevadia:** UAP, UFO, PURSUE ariv, dokumeni e hahedoatoa, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hanamona idia:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP ariv toana orea dekenai hereva momo, e diba search eia bona platform

**Laiv:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Source ariv:** https://www.war.gov/ufo

`ufolens.com` ese U.S. War Department ena **PURSUE** ariv, UAP / UFO rekodi e hahedoatoa, e parava lou hanai knowledge platform ta bamona: full-text search, hereva idau idau masini transleisen, map + taimlain eksploresen, bona pablik JSON API ta. Source dokumeni be U.S. federal gavaman ena gaukara bona U.S. lalonai be pablik domen ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Unai projek be **U.S. gavaman ida e hadibaia lasi**, ofisiel insignia e gaukaraia lasi, bona redaksen e ha-idau lasi.

### Akiteteksa

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

- **Ai Ai lou ese dokumeni ta ai cost lasi.** OCR bona transleisen be lokal ai e lao; forwad-oni state masini (`discovered → downloaded → ocr_done → translated → published`) ese e gwauhamata no dokumeni e raseproses lou lasi ke e idau.
- **Painlain kore ese tati pati dipendensi lasi** — pasing / manifes / delta mojul be Python bona no pip-insital lasi ai e lao bona e tes; OCR/transleisen steij be e heketi ke opesenel pekej be lasi.
- **Ej sait** ese sikiriti heda + CSP e gaukaraia (no `unsafe-inline`; inline JSON-LD sha256-pin); hereva tokana `Accept-Language` + kantri maping; 30 de KV peij kes; bona deilikron.
- **Inkirimenel apdeit:** delta detekta ese sors indeks e hadia bona idau eia hereva mauri painlain dekenai e hadibaia.

### Divelopa edia

https://www.ufolens.com/api/v1 ai pablik API ese dokumeni bona metada e henida JSON bamona. Anonimas akses be reit-limit; ki ta e noaia risesa/divelopa edia. Sait ena API seksen ai enpoin bona limit e itaia.

### Steitas

Kod komplet; sait e haroro https://www.ufolens.com. Prodaksen databes be oflain painlain e gaukaraia bona bundel e haroro forwad (`cli_publish run --remote`) bona e henuia. Ful disain dok be `docs/20260511/` ai.

### Laisens / banadis

- Sors dokumeni: U.S. federal gavaman gaukara, pablik domen U.S. lalonai.
- Dis platform ena kod: `LICENSE` itaia.
- Sait ese `Tdm-Reservation: 1` bona `X-Robots-Tag: noai, noimageai` e siaia — ses enjin ese e dabe diba, Ai trening/sraping lasi.
- Vido futej be DVIDS / AARO e haroa bona dis projek ese e noho lasi.

Isu bona PRs be e veria. `CLAUDE.md` bona `docs/20260511/00-*` e duia guna, guna strukurel senj e duia ma PR e haroro.

