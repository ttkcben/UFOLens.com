# GitHub — Ekiwandiko 1 kya 3 · Okulangirira Okutongole / Ebyokulangirira bya README

**Kozesa nga:** omubiri gwa GitHub Release, Okunyumya okusimbiddwa, oba waggulu ku README ya repo.
**Ebigambo ebikulu:** UAP, UFO, PURSUE archive, ebiwandiiko ebyasattululwa, data enzigule, okunoonya mu mawulire gonna, OCR, okuvvuunula kwa kompyuta, local LLM, Ollama, edge computing, API eya lwona, Hono, TypeScript, Python
**Enkolagana:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — olukalala olw'ennimi nnyingi, olusobola okunoonyezebwa olw'enkumu ya PURSUE UAP

**Luli ku:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Enkumu y'ensibuko:** https://www.war.gov/ufo

`ufolens.com` eddamu okutongoza enkumu ya **PURSUE** ey'ekitongole ky'olutalo eky'e U.S. ey'ebiwandiiko bya UAP / UFO ebyasattululwa ng'olukalala lw'amagezi: okunoonya mu mawulire gonna, okuvvuunula kwa kompyuta mu nkumu yonna, okuzuula ku maapu + ekiseera, n'ekitongole kya JSON API ekya lwona. Ebiwandiiko by'ensibuko bye bikolwa bya gavumenti ya U.S. era munda mwa U.S. biri mu lwona ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Pulojekiti eno **si nkolagana ne gavumenti ya U.S.**, tekozesa birango bitongole, era tesattulula bigambo ebyagibwamu.

### Enzimba

```
Kompyuta y'ewaka (Apple Silicon, IP y'ewaka)      Omutimbagano gw'oku nsalo
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   emiko
  OCR: enjini ya open-source (Tesseract CLI fallback)     /api/v1/...   API ya lwona
  translate / NER: local LLM (Gemma via Ollama)        /admin        ekifo ky'omukozesa
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── isulira omuganda: SQL + asset manifest + cache-purge list ──┘
```

- **Tewali muwendo gwonna ogwa AI ku buli kiwandiiko.** OCR n'okuvvuunula bikolera ku kompyuta y'ewaka; enkola y'okukola ekiddako yokka (`discovered → downloaded → ocr_done → translated → published`) ekakasa nti tewali kiwandiiko kikolebwako okuggyako nga kikyuse.
- **Omutima gwa Pipeline tegulina nkolagana na muntu wa kusatu** — ebitundu by'okugawanya / okutegeka / okukyusa bikola era bikeberwa ku Python enongoofu awatali kintu kyonna ekyateekebwako pip; ebitundu bya OCR/okuvvuunula bisobola okukola obulungi nga eby'okulondawo tebiriiwo.
- **Omukutu gw'oku nsalo** gukozesa amateeka ag'obukuumi amakakali + CSP (tewali `unsafe-inline`; inline JSON-LD esimbiddwa ku sha256), okutegeeragana kw'olulimi nga kuyita mu `Accept-Language` + okugatta ensi, ekifo kya KV cache eky'ennaku 30, n'okulongoosa okwa buli lunaku.
- **Okulongoosa okw'omugotteko:** akatundu akalaba enkyukakyuka kalondoola enkyukakyuka mu fayiro y'ensibuko ne kizzaayo enkyukakyuka zokka mu pipeline.

### Eri abakugu mu bya tekinologiya

API ya lwona ku https://www.ufolens.com/api/v1 ezzaayo ebiwandiiko n'ebirala nga JSON. Okuyingira awatali lukusa kulina ekkomo; saba olukusa okuyingira mu ddaala ly'abanoonyereza/abakugu. Laba ekitundu kya API ku mukutu okumanya enkomerero n'ebikwata ku bikugizo.

### Embeera

Koodi ewedde; omukutu gutaddwa ku https://www.ufolens.com. Database y'omukutu ejjuzibwa nga pipeline ekola awatali mutimbagano era n'esulira omuganda gwayo (`cli_publish run --remote`). Ebiwandiiko byonna ebikwata ku nteekateeka biri mu `docs/20260511/`.

### Layisinsi / Ebikugizo

- Ebiwandiiko by'ensibuko: bikolwa bya gavumenti ya U.S., mu lwona munda mwa U.S.
- Koodi y'olukalala luno yennyini: laba `LICENSE`.
- Omukutu gusindika `Tdm-Reservation: 1` ne `X-Robots-Tag: noai, noimageai` — enjini z'okunoonya zisobola okugiyingira, naye tegisobola kukozesebwa mu kutendeka AI/okukwata data.
- Obutambi bw'amavidiyo buvunanyizibwa ku DVIDS / AARO era tebuli mu pulojekiti eno.

Ebip new PRs byanirizibwa. Soma `CLAUDE.md` ne `docs/20260511/00-*` nga tonnaba kuggulawo nkyukakyuka nene.

