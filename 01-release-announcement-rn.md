# GitHub — Icahuri 1 c'ibitatu · Itangazo ryo gusohora / Agace k'itangazo ka README

**Koresha nka:** umubiri w'itangazo rya GitHub, ikiyago c'amanitswe, canke hejuru ya README y'ububiko.
**Amajambo y'ipfunguruzo:** UAP, UFO, ububiko bwa PURSUE, inyandiko zatangajwe, amakuru rusange, gushakisha inyandiko yuzuye, OCR, ubuhinduzi bwa mwikaryo, LLM yo muhira, Ollama, edge computing, API rusange, Hono, TypeScript, Python
**Amahuza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — urubuga rw'indimi nyinshi kandi rushobora gushakishwa rw'ububiko bwa PURSUE UAP

**Ruri ku murongo:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Ububiko bw'inkomoko:** https://www.war.gov/ufo

`ufolens.com` isubiramwo gusohora ububiko bwa **PURSUE** bw'Ishami ry'Ingwano rya Leta Zunze Ubumwe za Amerika bw'inyandiko za UAP / UFO zatangajwe nk'urubuga rw'ubumenyi: gushakisha inyandiko yuzuye, ubuhinduzi bwa mwikaryo mu bubiko bwose, gushakisha ku ikarita no ku murongo w'ibihe, hamwe n'API ya JSON rusange. Inyandiko z'inkomoko ni ibikorwa vya leta ya Amerika kandi muri Amerika ni umutungo rusange ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Uyu mugambi **ntaho uhuriye na leta ya Amerika**, ntukoresha ibimenyetso vyemewe, kandi ntiwigera uhindura ibyahishijwe.

### Ubwubatsi

```
Imashini yo muhira (Apple Silicon, IP yo mu rugo)   Umurongo w'impera (Edge network)
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, umutima wa stdlib-gusa)   worker/  (TypeScript, Hono.js)
  gukura → OCR → guhindura → gusohora (ija imbere gusa)    /{lang}/...   impapuro
  OCR: moteri y'inkomoko ifunguye (Tesseract CLI nk'insubirizi) /api/v1/...   API rusange
  guhindura / NER: LLM yo muhira (Gemma biciye kuri Ollama)   /admin        ikibaho c'umukoreshi
  ivyiciro: maniferesite ya SQLite                    ishingiye kuri: edge SQL DB, ububiko
        │                                              bw'ibintu (PDF z'inkomoko), cache ya KV
        └── isohora umugwi: SQL + maniferesite y'umutungo + urutonde rwo guhanagura cache ──┘
```

- **Ikiguzi c'ubusa ku nyandiko imwe imwe ku bijanye na AI yo ku gicu.** OCR n'ubuhinduzi bikorera muhira; imashini y'ivyiciro ija imbere gusa (`kivumbuwe → cakuwe → ocr_kirangiye → cahinduwe → casohotse`) yemeza ko ata nyandiko isubirwamwo kiretse yahindutse.
- **Umutima wa pipeline ntuva ku bandi bantu** — amamodule yo gucamwo ibice / maniferesite / delta akora kandi ageragezwa kuri Python isukuye ata na kimwe cashizwemwo na pip; ivyiciro vya OCR/ubuhinduzi birakora neza naho amapaki atari ngombwa aba adahari.
- **Urubuga rwo ku mpera** rushiramwo amategeko akomeye y'umutekano + CSP (ata `unsafe-inline`; inline JSON-LD ifise ikimenyetso ca sha256), guhuza indimi biciye kuri `Accept-Language` + guhuza igihugu, cache y'impapuro imara imisi 30 kuri KV, hamwe n'igikorwa ca cron c'isuku ca minsi yose.
- **Ukuvugurura gake gake:** igikoresho kiraba itandukaniro giharura itandukaniro ry'urutonde rw'inkomoko kigaha gusa impinduka pipeline.

### Ku bakora porogaramu

API rusange kuri https://www.ufolens.com/api/v1 itanga inyandiko n'amakuru y'inyongera nka JSON. Abadakoresha amazina baragirirwa imipaka; saba urupfunguruzo ku rwego rw'abashakashatsi/abakora porogaramu. Raba igice c'API ku rubuga kugira umenye aho biherera n'imipaka.

### Uko bihagaze

Kode yarangiye; urubuga rwashizwe kuri https://www.ufolens.com. Ububiko bw'amakuru bwo mu buhinga bwuzuzwa mu gukoresha pipeline itari ku murongo no gusohora umugwi imbere (`cli_publish run --remote`). Inyandiko zuzuye z'umugambi ziri muri `docs/20260511/`.

### Urwandiko rw'uburenganzira / Imipaka

- Inyandiko z'inkomoko: ibikorwa vya leta ya Amerika, umutungo rusange muri Amerika.
- Kode y'uru rubuga nyene: raba `LICENSE`.
- Urubuga rurungika `Tdm-Reservation: 1` na `X-Robots-Tag: noai, noimageai` — rushobora gushakishwa n'imashini zishakisha, rwakuyemwo inyigisho za AI/gukurura amakuru.
- Amashusho y'amavidewo yavanywe kuri DVIDS / AARO kandi ntaho ahuriye n'uyu mugambi.

Ibibazo na PRs murahawe ikaze. Musabwe gusoma `CLAUDE.md` na `docs/20260511/00-*` imbere yo gutanguza impinduka zikomeye.

