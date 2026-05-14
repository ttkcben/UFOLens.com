# GitHub — Post 1 o 3 · Bloc cyhoeddiad Rhyddhau / README

**Defnyddiwch fel:** corff Datganiad GitHub, Trafodaeth wedi'i phinio, neu frig README y repo.
**Allweddeiriau:** UAP, UFO, archif PURSUE, dogfennau wedi'u dat-ddosbarthu, data agored, chwiliad testun llawn, OCR, cyfieithu peirianyddol, LLM lleol, Ollama, cyfrifiadura ymylol, API cyhoeddus, Hono, TypeScript, Python
**Hypergysylltiadau:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — platfform amlieithog, chwilioadwy ar gyfer archif UAP PURSUE

**Yn fyw:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archif ffynhonnell:** https://www.war.gov/ufo

Mae `ufolens.com` yn ail-gyhoeddi archif **PURSUE** Adran Ryfel yr U.D. o gofnodion UAP / UFO sydd wedi'u dat-ddosbarthu fel platfform gwybodaeth: chwiliad testun llawn, cyfieithu peirianyddol ar draws y corws, archwilio map + llinell amser, ac API JSON cyhoeddus. Mae dogfennau ffynhonnell yn weithiau llywodraeth ffederal yr U.D. ac o fewn yr U.D. maent yn y parth cyhoeddus ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). **Nid yw'r prosiect hwn yn gysylltiedig â llywodraeth yr U.D.**, nid yw'n defnyddio unrhyw arwyddluniau swyddogol, a byth yn gwrthdroi golygiadau.

### Pensaernïaeth

```
Peiriant lleol (Apple Silicon, IP preswyl)           Rhwydwaith ymylol
──────────────────────────────────────────          ──────────────────────────
pipeline/  (Python 3.10, craidd stdlib-yn-unig)      worker/  (TypeScript, Hono.js)
  adalw → OCR → cyfieithu → cyhoeddi  (ymlaen-yn-unig)   /{lang}/...   tudalennau
  OCR: peiriant ffynhonnell agored (Tesseract CLI wrth gefn) /api/v1/...   API cyhoeddus
  cyfieithu / NER: LLM lleol (Gemma drwy Ollama)       /admin        consol gweithredwr
  cyflwr: maniffesto SQLite                          wedi'i gefnogi gan: DB SQL ymylol, storfa
        │                                              gwrthrychau (PDFs ffynhonnell), storfa KV
        └── yn cyhoeddi bwndel: SQL + maniffesto asedau + rhestr glanhau storfa ──┘
```

- **Dim cost cwmwl-AI fesul dogfen.** Mae OCR a chyfieithu'n rhedeg yn lleol; mae'r peiriant cyflwr ymlaen-yn-unig (`darganfod → lawrlwytho → ocr_wedi'i_wneud → cyfieithu → cyhoeddi`) yn gwarantu na chaiff unrhyw ddogfen ei hailbrosesu oni bai ei bod wedi newid.
- **Nid oes gan graidd y biblinell unrhyw ddibyniaethau trydydd parti** — mae modiwlau dosrannu / maniffesto / delta yn rhedeg ac yn profi ar Python glân heb ddim wedi'i osod â pip; mae camau OCR/cyfieithu yn israddio'n osgeiddig pan fydd pecynnau dewisol yn absennol.
- **Mae safle ymylol** yn cymhwyso penawdau diogelwch llym + CSP (dim `unsafe-inline`; JSON-LD mewnol wedi'i binio â sha256), negodi iaith drwy `Accept-Language` + mapio gwledydd, storfa dudalen KV 30 diwrnod, a chron cadw tŷ dyddiol.
- **Diweddariadau cynyddrannol:** mae synhwyrydd delta yn cymharu mynegai'r ffynhonnell ac yn bwydo newidiadau'n unig yn ôl i'r biblinell.

### Ar gyfer datblygwyr

Mae'r API cyhoeddus yn https://www.ufolens.com/api/v1 yn dychwelyd dogfennau a metadata fel JSON. Mae mynediad dienw wedi'i gyfyngu o ran cyfradd; gofynnwch am allwedd ar gyfer haenau ymchwilydd/datblygwr. Gweler adran API y safle am ddiweddbwyntiau a therfynau.

### Statws

Cod wedi'i gwblhau; safle wedi'i ddefnyddio yn https://www.ufolens.com. Mae'r gronfa ddata gynhyrchu yn cael ei phoblogi drwy redeg y biblinell all-lein a chyhoeddi'r bwndel ymlaen (`cli_publish run --remote`). Mae dogfennau dylunio llawn yn byw yn `docs/20260511/`.

### Trwydded / ffiniau

- Dogfennau ffynhonnell: Gweithiau llywodraeth ffederal yr U.D., parth cyhoeddus o fewn yr U.D.
- Cod y platfform hwn ei hun: gweler `LICENSE`.
- Mae'r safle'n anfon `Tdm-Reservation: 1` ac `X-Robots-Tag: noai, noimageai` — yn fynegeiadwy gan beiriannau chwilio, wedi'i optio allan o hyfforddiant/crafu AI.
- Priodolir deunydd fideo i DVIDS / AARO ac nid yw'n cael ei hawlio gan y prosiect hwn.

Croeso i Issues a PRs. Darllenwch `CLAUDE.md` a `docs/20260511/00-*` cyn agor newidiadau strwythurol.
