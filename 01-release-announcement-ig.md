# GitHub — Akwụkwọ 1 n'ime 3 · Nkwupụta Mwepụta / README

**Jiri dị ka:** Isi akwụkwọ GitHub Release, Mkparịta ụka a kapịrị ọnụ, ma ọ bụ n'elu ebe a na-ede README.
**Okwu Igodo:** UAP, UFO, PURSUE archive, akwụkwọ ndị ewepụtara, data mepere emepe, ọchụchọ ederede zuru ezu, OCR, ntụgharị igwe, LLM mpaghara, Ollama, mgbakọ na nsọtụ, API ọha, Hono, TypeScript, Python
**Njikọ:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — ikpo okwu asụsụ dị iche iche, nke a na-enyocha maka ebe nchekwa PURSUE UAP

**N'ịntanetị:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Ebe nchekwa isi:** https://www.war.gov/ufo

`ufolens.com` na-ebipụtaghachi ebe nchekwa **PURSUE** nke Ngalaba Agha US maka ndekọ UAP / UFO ewepụtara dị ka ikpo okwu ihe ọmụma: ọchụchọ ederede zuru ezu, ntụgharị igwe n'ofe mkpokọta ahụ, maapụ + nyocha usoro oge, na JSON API ọha. Akwụkwọ isi mmalite bụ ọrụ gọọmentị etiti US na n'ime US bụ ngalaba ọha ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ọrụ a **ejikọtaghị ya na gọọmentị US**, anaghị eji akara ọ bụla, ọ dịghịkwa mgbe ọ na-atụgharị mmegharị.

### Nhazi

```
Igwe mpaghara (Apple Silicon, IP obibi)              Netwọk dị na nsọtụ
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   ibe
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   API ọha
  translate / NER: local LLM (Gemma via Ollama)        /admin        njikwa onye ọrụ
  state: SQLite manifest                             nke a kwadoro: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── na-ebipụta ngwugwu: SQL + asset manifest + cache-purge list ──┘
```

- **Ọnụ ego efu maka igwe ojii-AI kwa akwụkwọ.** OCR na ntụgharị na-agba ọsọ na mpaghara; igwe steeti na-aga n'ihu (`discovered → downloaded → ocr_done → translated → published`) na-ekwe nkwa na a gaghị edozi akwụkwọ ọ bụla ọzọ belụsọ ma ọ gbanwere.
- **Isi pipeline enweghị ndabere nke atọ** — modul nyocha / ngosipụta / delta na-agba ọsọ ma na-anwale na Python dị ọcha na-enweghị ihe ọ bụla arụnyere; usoro OCR/ntụgharị na-eweda ala nke ọma mgbe ngwugwu nhọrọ adịghị.
- **Saịtị dị na nsọtụ** na-etinye nkụnye eji isi mee nchekwa siri ike + CSP (enweghị `unsafe-inline`; inline JSON-LD sha256-pinned), mkparịta ụka asụsụ site na `Accept-Language` + maapụ obodo, cache ibe KV ụbọchị 30, na cron na-arụ ọrụ kwa ụbọchị.
- **Mmelite na-aga n'ihu:** onye na-achọpụta delta na-ahụ ọdịiche dị na ndeksi isi iyi ma na-enye naanị mgbanwe azụ na pipeline.

### Maka ndị mmepe

API ọha na https://www.ufolens.com/api/v1 na-eweghachi akwụkwọ na metadata dị ka JSON. A na-egbochi ohere amaghị aha; rịọ igodo maka ọkwa onye nyocha/onye nrụpụta. Hụ ngalaba API na saịtị ahụ maka njedebe na oke.

### Ọnọdụ

Koodu zuru ezu; saịtị ebugara na https://www.ufolens.com. A na-ejuputa nchekwa data mmepụta site na ịgba ọsọ pipeline na-anọghị n'ịntanetị na ibipụta ngwugwu ahụ n'ihu (`cli_publish run --remote`). Akwụkwọ nhazi zuru ezu dị na `docs/20260511/`.

### Akwụkwọ ikike / oke

- Akwụkwọ isi iyi: ọrụ gọọmentị etiti US, ngalaba ọha n'ime US.
- Koodu ikpo okwu a: lee `LICENSE`.
- Saịtị ahụ na-eziga `Tdm-Reservation: 1` na `X-Robots-Tag: noai, noimageai` — nke igwe nchọta nwere ike ịdepụta, ewepụrụ na ọzụzụ AI/scraping.
- A na-ekwu na ihe nkiri vidiyo sitere na DVIDS / AARO na ọrụ a anaghị ekwu na ọ bụ ya.

A na-anabata nsogbu na PR. Biko gụọ `CLAUDE.md` na `docs/20260511/00-*` tupu imepe mgbanwe nhazi.
