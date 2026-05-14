# GitHub — Nsangu 1 ya 3 · Lusansu ya Kuyidika / Nsangu ya README

**Sadila bonso:** n'kanda ya GitHub Release, mosi ya Discussion ya bo me kangisa, to na zulu ya README ya kisika ya kubaka.
**Mvovo ya mfunu:** UAP, UFO, arsiv ya PURSUE, mikanda ya bo me katula ya n'sokono, data ya me fionguna, nsosa ya nkanda ya mvimba, OCR, nbalula ya masini, LLM ya kisika, Ollama, edge computing, API ya kimvwama, Hono, TypeScript, Python
**Bikwati:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — n'kua ya bandinga mingi, ya kusosa sambu na arsiv ya PURSUE UAP

**Na ntoto:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Arsiv ya kisina:** https://www.war.gov/ufo

`ufolens.com` ke basisaka diaka arsiv ya **PURSUE** ya Departema ya Vita ya Amerika ya mikanda ya UAP / UFO ya bo me katula n'sokono bonso n'kua ya nzayilu: nsosa ya nkanda ya mvimba, nbalula ya masini na kati ya arsiv, nsosa na karte mpi na ntangu, mpi API ya JSON ya kimvwama. Mikanda ya kisina kele bisalu ya luyalu ya États-Unis mpi na kati ya États-Unis yo kele na kimvwama ya bantu yonso ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Proje yai kele **ve ya kuwakana ti luyalu ya États-Unis**, yo ke sadilaka ve bidimbu ya luyalu, mpi yo ke vutulaka ve mikanda ya bo me fukisa.

### Mfumo ya Kutunga

```
Masini ya kisika (Apple Silicon, IP ya nzo)        Reso ya nsongi
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, ntima ya stdlib mpamba)     worker/  (TypeScript, Hono.js)
  bonga → OCR → balula → basisa  (na ntwala mpamba)    /{lang}/...   balupangu
  OCR: motere ya me fionguna (Tesseract CLI ya kusadila kana ya ntete kele ve)     /api/v1/...   API ya kimvwama
  balula / NER: LLM ya kisika (Gemma via Ollama)        /admin        konsoli ya muntu ya ke salaka
  kima ya kele: manifeste ya SQLite                             ya me simbama na: base de données SQL ya nsongi, kisika ya kubumba bima
        │                                              (ba PDF ya kisina), cache ya KV
        └── basisaka kitini: SQL + manifeste ya bima + lisiti ya kufimpasa cache ──┘
```

- **Nge ke futaka ve ata kima mosi na AI ya matata sambu na konso nkanda.** OCR mpi nbalula ke salamaka na kisika; masini ya ke kwendaka kaka na ntwala (`me monana → me telecharger → ocr_done → me balula → me basisa`) ke pesaka nsilulu nde bo ke vutukilaka ve nkanda yina bo me bonga kana yo me sobaka ve.
- **Ntima ya pipeline kele ve ti ata dependance mosi ya muntu ya tatu** — ba modules ya kutanga / manifeste / delta ke kwendaka mpi ke mekama na Python ya peto ya me telama ve ti ata kima mosi ya pip; bitini ya OCR/nbalula ke landaka kusala mbote kana bapaketi ya bo ke ponaka kele ve.
- **Site ya nsongi** ke tulaka ba en-têtes ya securite ya ngolo + CSP (ata `unsafe-inline` ve; JSON-LD ya kati ya kanda kele ya me kangama na sha256), nkubu ya ndinga na nzila ya `Accept-Language` + karte ya bansi, cache ya KV ya bilumbu 30, mpi cron ya kuyidika bima konso kilumbu.
- **Masantu ya malembe-malembe:** muntu ya ke monaka bansoba ke talaka nsasa ya index ya kisina mpi ke pesaka kaka bansoba na pipeline.

### Sambu na bantu ya ke salaka ba programe

API ya kimvwama na https://www.ufolens.com/api/v1 ke vutulaka mikanda mpi metadata na JSON. Bantu ya kele ve ti nkumbu kele ti ndilu ya kusadila; lomba nsabi sambu na bantu ya ke salaka bansosa/bantu ya ke salaka ba programe. Tala kitini ya API na site sambu na bantwala mpi bandilu.

### Kima ya kele

Code me mana; site me telama na https://www.ufolens.com. Base de données ya kubasisa ke yelaka na kusadisa pipeline ya kele ve na internet mpi na kubasisa kitini na ntwala (`cli_publish run --remote`). Mikanda yonso ya plan kele na `docs/20260511/`.

### Lisansi / Bandilu

- Mikanda ya kisina: Bisalu ya luyalu ya États-Unis, kimvwama ya bantu yonso na kati ya États-Unis.
- Code ya n'kua yai: tala `LICENSE`.
- Site ke tindaka `Tdm-Reservation: 1` mpi `X-Robots-Tag: noai, noimageai` — yo kele sambu na bamotere ya nsosa, yo me buya nde AI kubaka mpi kusadila yo.
- Ba video kele ya DVIDS / AARO mpi kele ve ya proje yai.

Mambu ya mpasi mpi PRs kele ya kuluta mbote. Tanga `CLAUDE.md` mpi `docs/20260511/00-*` na ntwala ya kukangula bansoba ya nene.
