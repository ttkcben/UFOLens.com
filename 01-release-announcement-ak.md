# GitHub — Nkrataa 1 a ɛwɔ 3 mu · Ntoada/README nkrata tom
**Fa di dwuma sɛ:** GitHub Release nkrata, Nkitahodi a wɔde ahyɛ hɔ, anaasɛ repo README ti.
**Nneɛma a ɛho hia:** UAP, UFO, PURSUE kyerɛwtohɔ, nkrata a wɔda no adi, data a ɛda adi, nsɛm a wɔhwehwɛ mu, OCR, mfiridwuma mu nkyerɛase, LLM a ɛwɔ mpɔtam hɔ, Ollama, edge computing, API a ɛda adi, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — kasa ahodoɔ pii, beae a wɔhwehwɛ mu ma PURSUE UAP kyerɛwtohɔ

**Te ase:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Nkrata a ɛhyɛ ase:** https://www.war.gov/ufo

`ufolens.com` san tintim U.S. Akode Tuo Dwumadibea no **PURSUE** kyerɛwtohɔ a ɛfa UAP / UFO ho nkrata a wɔada no adi sɛ nimdeɛ beae: nsɛm a wɔhwehwɛ mu, mfiridwuma mu nkyerɛase a ɛfa nkrata no nyinaa ho, asase mfonini + berɛ nhwehwɛmu, ne JSON API a ɛda adi. Nkrata a ɛhyɛ aseɛ no yɛ U.S. aban adwuma na ɛwɔ U.S. no, ɛyɛ amansan dea ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Adwuma yi **nni abusuabɔ biara ne U.S. aban**, ɛnni aban agyiraehyɛde biara, na ɛnsan nkyerɛ nkrata a wɔde ahintaw no bio da.

### Nnwumadie Nhyehyɛe

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

- **Sika a wɔbɔ wo wɔ cloud-AI so no nni hɔ koraa wɔ krataa biara a wɔyɛ ho adwuma no ho.** OCR ne nkyerɛaseɛ yɛ adwuma wɔ mpɔtam hɔ; `discovered → downloaded → ocr_done → translated → published` a ɛkɔ anim nko ara no ma ɛyɛ den sɛ wɔbɛsan ayɛ krataa bi ho adwuma gye sɛ ɛsesa.
- **Pipeline core no nni ankorankoro mmoa biara** — parsing / manifest / delta modules no yɛ adwuma na wɔsɔ hwɛ wɔ Python a ɛho tew a biribiara nni pip-installed mu; OCR/translation gyinapɛn no brɛ ase yiye bere a packages a wotumi paw no nni hɔ no.
- **Edge site** no de ahobammɔ ho nsɛm a ɛyɛ den + CSP di dwuma (nni `unsafe-inline`; inline JSON-LD sha256-pinned), kasa mu nkitahodi a ɛnam `Accept-Language` + ɔman ntam nkitahodi so, KV krataa a wɔkora so nnafua 30, ne da biara da fie adwuma cron.
- **Nsesaeɛ a ɛkɔ so nkakrankakra:** delta detector hu nsesaeɛ a ɛba source index no mu na ɛde nsesaeɛ no san de kɔ pipeline no mu.

### Ma wɔn a wɔyɛ adwuma no

API a ɛda adi wɔ https://www.ufolens.com/api/v1 no de nkrata ne metadata ma sɛ JSON. Ankorankoro a wɔnnim wɔn no, wɔn kwan yɛ tiaa; srɛ key ma nhwehwɛmufoɔ/developerfoɔ gyinapɛn. Hwɛ API fã a ɛwɔ beaeɛ no so ma endpoints ne anohyetoɔ.

### Gyinabea

Code awie; wɔde beaeɛ no ato hɔ wɔ https://www.ufolens.com. Wɔde pipeline a ɛnni intanɛt so na ɛyɛ adwuma no na ɛma production database no yɛ ma (`cli_publish run --remote`). Nkrata a ɛkyerɛkyerɛ adwuma no nyinaa mu no wɔ `docs/20260511/`.

### Laisense / anohyetoɔ

- Nkrata a ɛhyɛ aseɛ: U.S. aban adwuma, amansan dea wɔ U.S.
- Beaeɛ yi code no ankasa: hwɛ `LICENSE`.
- Beaeɛ no de `Tdm-Reservation: 1` ne `X-Robots-Tag: noai, noimageai` mena — search engines tumi hwehwɛ mu, nanso wɔayi wɔn ho afi AI nteteeɛ/scraping mu.
- Wɔde video no ma DVIDS / AARO na adwuma yi nka sɛ ɛyɛ wɔn dea.

Nsɛm ne PRs nyinaa fata. Yɛsrɛ wo kenkan `CLAUDE.md` ne `docs/20260511/00-*` ansa na woabue nsesaeɛ biara a ɛfa nhyehyɛeɛ ho.

