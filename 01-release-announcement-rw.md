# GitHub — Ingingo 1 kuri 3 · Itangazo ryo gusohora / README

**Rikoreshwe nka:** umubiri w'itangazo ryo gusohora kuri GitHub, Ikiganiro gihamye, cyangwa hejuru ku README y'ububiko.
**Amagambo y'ingenzi:** UAP, UFO, ububiko bwa PURSUE, inyandiko zatangajwe, amakuru afunguye, gushakisha inyandiko zose, OCR, ubusemuzi bw'imashini, LLM y'imbere mu gihugu, Ollama, edge computing, API rusange, Hono, TypeScript, Python
**Imiyoboro:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — urubuga rw'indimi nyinshi kandi rushobora gushakishwamo rw'ububiko bwa PURSUE UAP

**Kuri murandasi:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Ububiko bw'inkomoko:** https://www.war.gov/ufo

`ufolens.com` yongera gusohora ububiko bwa **PURSUE** bw'Ishami ry'Intambara rya Leta Zunze Ubumwe z’Amerika bw'inyandiko zatangajwe za UAP / UFO nkurubuga rw'ubumenyi: gushakisha inyandiko zose, ubusemuzi bw'imashini mu bubiko bwose, gushakisha ku ikarita n'igihe, hamwe na API rusange ya JSON. Inyandiko z'inkomoko ni ibihangano bya leta ya Leta Zunze Ubumwe z’Amerika kandi muri Leta Zunze Ubumwe z’Amerika biri mu mutungo rusange ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Uyu mushinga **ntabwo ufite isano na leta ya Leta Zunze Ubumwe z’Amerika**, ntukoresha ibirango bya leta, kandi ntiwigera uhindura ibyahishwe.

### Imiterere y'ubwubatsi

```
Imashini y'imbere mu gihugu (Apple Silicon, IP yo mu rugo)     Umuyoboro w'impera (Edge network)
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, umutima ukoresha stdlib gusa)   worker/  (TypeScript, Hono.js)
  gukurura → OCR → gusemura → gusohora (mu cyerekezo kimwe)  /{lang}/...   impapuro
  OCR: moteri ya open-source (Tesseract CLI nk'uburyo bwisumbuye) /api/v1/...   API rusange
  gusemura / NER: LLM y'imbere mu gihugu (Gemma binyuze kuri Ollama)   /admin        konsole y'umukoresha
  imiterere: Manifesite ya SQLite                        ishingiye kuri: banki y'amakuru ya SQL y'impera, ububiko
        │                                              bw'ibintu (inyandiko za PDF z'inkomoko), cache ya KV
        └── isohora umuzingo: SQL + manifesite y'umutungo + urutonde rwo gusiba cache ──┘
```

- **Igiciro cya zeru kuri buri nyandiko cy'ubwenge buhimbano bwo mu gicu.** OCR n'ubusemuzi bikorerwa imbere mu gihugu; imashini y'imiterere igana imbere gusa (`yavumbuwe → yakuweho → ocr_yakozwe → yasemuwe → yasohowe`) yemeza ko nta nyandiko yongera gukorwaho keretse niba yahindutse.
- **Umutima w'umuyoboro nta bintu by'abandi wishingikirijeho** — uburyo bwo gusesengura / manifesite / delta bikora kandi bigeragezwa kuri Python isukuye idafite ikintu na kimwe cyashyizwemo na pip; ibyiciro bya OCR/ubusemuzi bipfa buhoro buhoro iyo porogaramu z'inyongera zidahari.
- **Urubuga rw'impera** rukoresha uburyo bukomeye bw'umutekano + CSP (nta `unsafe-inline`; JSON-LD yo mu murongo yashyizweho ikimenyetso cya sha256), guhitamo ururimi binyuze kuri `Accept-Language` + guhuza n'igihugu, cache y'urupapuro ya KV y'iminsi 30, na cron yo gusukura ya buri munsi.
- **Ivugurura ry'ibyiyongera:** igikoresho cyo gutahura delta kigereranya urutonde rw'inkomoko kandi kigaburira gusa impinduka mu muyoboro.

### Ku bakoresha porogaramu

API rusange kuri https://www.ufolens.com/api/v1 itanga inyandiko n'amakuru y'inyongera nka JSON. Abinjira batamenyekanye bafite umubare ntarengwa w'ibyo bakora; saba urufunguzo rw'ibyiciro by'abashakashatsi/abakora porogaramu. Reba igice cya API ku rubuga kugira ngo umenye aho wakura amakuru n'imipaka.

### Imiterere

Kode yarangiye; urubuga rwashyizwe kuri https://www.ufolens.com. Banki y'amakuru y'umusaruro yuzuzwa mu gukoresha umuyoboro wo hanze no gusohora umuzingo ujya imbere (`cli_publish run --remote`). Inyandiko zose z'imiterere ziri muri `docs/20260511/`.

### Uruhushya / Imipaka

- Inyandiko z'inkomoko: Ibihangano bya leta ya Leta Zunze Ubumwe z’Amerika, biri mu mutungo rusange muri Leta Zunze Ubumwe z’Amerika.
- Kode y'uru rubuga ubwarwo: reba `LICENSE`.
- Urubuga rwohereza `Tdm-Reservation: 1` na `X-Robots-Tag: noai, noimageai` — rushobora gushakishwa n'imashini zishakisha, ariko rwavanywemo mu myitozo/gukusanya amakuru by'ubwenge buhimbano.
- Amashusho y'amavidewo yitirirwa DVIDS / AARO kandi ntabwo ari ay'uyu mushinga.

Ibibazo na PRs byakiranwa yombi. Nyamuneka soma `CLAUDE.md` na `docs/20260511/00-*` mbere yo gutangiza impinduka z'imiterere.

