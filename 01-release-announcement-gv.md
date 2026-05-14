# GitHub — Post 1 jeh 3 · Fogrey Scughee / Block fogrey README

**Ymmyd:** myr corpys Scughee GitHub, Resooneyaght festit, ny ec kione y README y vinnid.
**Focklyn-ogher:** UAP, UFO, tasht-fysseree PURSUE, docamadyn neuvrónit, data foshlit, ronsaghey-lane-teks, OCR, çhyndaa-claare, LLM ynnydagh, Ollama, co-earrooaghey oir, API theayagh, Hono, TypeScript, Python
**Hyperchianglaghyn:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — ardane yl-hengagh, ronsaghey-ys son tasht-fysseree PURSUE UAP

**Bio:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Tasht-fysseree bunneydagh:** https://www.war.gov/ufo

`ufolens.com` t'eh aa-hoilshaghey magh tasht-fysseree **PURSUE** jeh Rheynn Caggee ny Steatyn Unnaneysit jeh recortyssyn neuvrónit UAP / UFO myr ardane oayllys: ronsaghey-lane-teks, çhyndaa-claare tessen y chorpys, ronsaghey-caslys-çheerey + linney-traa, as API JSON theayagh. Ta docamadyn bunneydagh nyn obbraghyn jeh reiltys conastagh ny SU as, ayns ny SU, t'ad 'sy rheam theayagh ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Cha nel y shaleeys shoh **co-chianglt rish reiltys ny SU**, cha nel eh jannoo ymmyd jeh cowraghyn oikoil, as cha nel eh rieau cur er ash ardoilghyn.

### Ard-obbrinys

```
Jeshaght ynnydagh (Apple Silicon, IP beaghee)      Greasane oir
──────────────────────────────────────────          ──────────────────────────
pipeline/  (Python 3.10, cree stdlib-ynrican)      worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (er oaie-ynrican)    /{lang}/...   duillagyn
  OCR: jeshaght foshlit-foshlit (Tesseract CLI ergooyl)     /api/v1/...   API theayagh
  translate / NER: LLM ynnydagh (Gemma trooid Ollama)      /admin        coonceil obbraidagh
  stayd: manifest SQLite                                 goaill back rish: DB SQL oir, stoyral
        │                                                  nhee (PDFyn bunneydagh), tasht KV
        └── t'eh soilshaghey magh bundeil: SQL + manifest cooid + rolley glenney-tasht ──┘
```

- **Costas ooilley-AI gagh docamad mannagh vel veg.** Ta OCR as çhyndaa-claare roie dy ynnydagh; ta'n jeshaght stayd er oaie-ynrican (`feddynit → lhieedynit → ocr_jeant → çhyndaait → soilshit magh`) cur barrantys nagh vel docamad erbee goll er aa-obbraghey mannagh ren eh caghlaa.
- **Cha nel croghaneyssyn trass-phartee ec cree y phib-linney** — ta modjulyn parseil / manifest / delta roie as prowal er Python glen gyn veg pip-vunnit; ta keimyn OCR/çhyndaa-claare leodaghey dy graysoil tra ta paggaghyn reihyssagh ass.
- **Ynnyd-eggey oir** t'eh cur kione-focklyn+CSP (gyn `unsafe-inline`; JSON-LD 'sy linney sha256-festit) shickyraght geyre, co-chruinnaghey çhengey trooid `Accept-Language` + caslys-çheerey çheer, tasht duillag KV 30-laa, as cron obbyr-thie laaoil.
- **Aasaghey-syn ynsit:** ta lorgeyder delta cur gerrymnane er yn ayrdex bunneydagh as t'eh cur stiagh caghlaaghyn ynrican ergooyl 'sy phib-linney.

### Son lhiassoyderyn

Ta'n API theayagh ec https://www.ufolens.com/api/v1 cur er ash docamadyn as metadata myr JSON. Ta access gyn ennym currit fo chagliagh; yeearr ogher son keimyn ronseyder/lhiasseyder. Jeeagh er y rheynn API er yn ynnyd-eggey son poyntyn jerree as cagliaghyn.

### Stayd

Coad creaghnit; ynnyd-eggey currit magh ec https://www.ufolens.com. Ta'n stoyr-fysseree gientynys currit rish liorish roie y phib-linney mooie-linney as soilshaghey magh y bundeil er oaie (`cli_publish run --remote`). Ta docamadyn cummey lane cummal ayns `docs/20260511/`.

### Kiedoonys / cagliaghyn

- Docamadyn bunneydagh: Obbraghyn reiltys conastagh ny SU, 'sy rheam theayagh ayns ny SU.
- Coad yn ardane hene: jeeagh er `LICENSE`.
- Ta'n ynnyd-eggey cur `Tdm-Reservation: 1` as `X-Robots-Tag: noai, noimageai` — so-ayrdexagh liorish jeshaghtyn ronsaghey, reihit magh ass traenal/scraapal AI.
- Ta filmmyn currit sheese da DVIDS / AARO as cha nel ad goit rish liorish y shaleeys shoh.

Ta skeealyn as PRyn failtit. Lhaih `CLAUDE.md` as `docs/20260511/00-*` roish my gowee shiu foshley caghlaaghyn strughtooragh.

