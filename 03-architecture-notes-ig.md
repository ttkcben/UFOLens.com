# GitHub — Akwụkwọ 3 n'ime 3 · Ihe ndekọ nhazi (Mkparịta ụka ụdị ADR)

**Jiri dị ka:** Mkparịta ụka n'okpuru "Gosi na ịkọ" / "Nhazi", ma ọ bụ `docs/` mkpụrụ ADR.
**Okwu Igodo:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Njikọ:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ihe mere eji wu ufolens.com n'ụzọ o si dị

Ihe ndetu na mkpebi atọ ahụ kpụrụ [ufolens.com](https://www.ufolens.com) (nke a na-enyocha, nke nwere ọtụtụ asụsụ nke [PURSUE UAP archive](https://www.war.gov/ufo)). A na-anabata nkwupụta / nkwughachi.

### 1. Pipeline bụ igwe steeti na-aga n'ihu - na ebumnuche

Steeti: `discovered → downloaded → ocr_done → translated → published`. Akwụkwọ na-aga n'ihu naanị, naanị mgbe enwere ọrụ a ga-arụ. A naghị edozi ọdịnaya ebipụtara ọzọ belụsọ ma onye na-achọpụta delta ahụla isi mmalite agbanweela n'ezie.

**Ihe kpatara ya:** OCR + ntụgharị bụ ọrụ dị oke ọnụ, na ebe nchekwa na-eto ka oge na-aga. Pipeline nke "na-agbagharị ihe niile ka ọ dị mma" nwere ọnụ ahịa na-akparaghị ókè. Ime ka mgbanwe azụ ghara ikwe omume na-eme ka ụgwọ na-agba ọsọ ghara ikwe omume. Elu ụlọ bụ ihe onwunwe nke eserese steeti, ọ bụghị nke nlebara anya onye ọrụ.

**Ọnụ ego:** mbugharị schema na nhazigharị na-ebumnuche bụ ihe na-adịghị mma. Azụmahịa a na-anabata.

### 2. OCR na ntụgharị na-agba ọsọ na LLM mpaghara, ọ bụghị API igwe ojii

OCR: injin mepere emepe, Tesseract CLI fallback. Ntụgharị + NER: Gemma site na Ollama, na laptọọpụ Apple Silicon.

**Ihe kpatara ya:** efu efu efu kwa akwụkwọ; a na-emegharị ya (ụdị edozi + kpaliri); na nzọụkwụ nbudata ahụ ga-agbarịrị site na IP ebe obibi (isi iyi dị n'azụ Akamai Bot Manager — `curl` na-enweta 403), yabụ laptọọpụ dị na loop agbanyeghị.

**Ọnụ ego:** ogo ntụgharị dị n'okpuru ụdị oke. Maka corpus ntụaka ebe Bekee izizi na-abụkarị otu ọpịpị, nke ahụ dị mma. Anyị anaghị ekwu na ntụgharị asụsụ ndị ahụ bụ ikike.

### 3. Akụkụ abụọ ahụ na-ekerịta otu interface: ngwugwu ebipụtara

Pipeline anaghị edegara nchekwa data mmepụta ozugbo. Ọ na-ewepụta `{ SQL, asset manifest, cache-purge list }`. "Ibipụta" = tinye ngwugwu ahụ n'ihu (kwanye SQL na nchekwa data SQL dị na nsọtụ, mekọrịta akụ na nchekwa ihe, kpochapụ igodo cache aha).

**Ihe kpatara ya:** akụkụ mpaghara na akụkụ nsọtụ nwere ike itolite n'onwe ha; ngwugwu a na-enyocha; na "data ebuga" bụ otu ụdị oge ọ bụla. Onye ọrụ bụ obere ngwa TypeScript/Hono — CSP siri ike (enweghị `unsafe-inline`; inline JSON-LD bụ sha256-pinned), `Accept-Language` + mkparịta ụka obodo → asụsụ, cache ibe KV ụbọchị 30, cron na-arụ ọrụ kwa ụbọchị — ọ dịghịkwa mkpa ịma ka esi eme data ahụ.

**Ọnụ ego:** mgbanwe atụmatụ D1 na-emetụ faịlụ abụọ aka (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Inshọransị dị ọnụ ala.

### Ihe anaghị ekwe omume agbakwunyere n'omume

- Ejikọtaghị ya na gọọmentị US; enweghị akara gọọmentị.
- A na-echekwa mmegharị isi mmalite, ọ dịghị mgbe a na-atụgharị ya.
- Vidio e kwuru sitere na DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` saịtị niile — enwere ike ịdepụta ọchụchọ, ewepụrụ na ncha AI.

N'ịntanetị: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
