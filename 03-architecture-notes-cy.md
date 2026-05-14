# GitHub — Post 3 o 3 · Nodiadau pensaernïaeth (Trafodaeth ar ffurf ADR)

**Defnyddiwch fel:** Trafodaeth o dan "Dangos a dweud" / "Pensaernïaeth", neu hedyn ADR ar gyfer `docs/`.
**Allweddeiriau:** pensaernïaeth, ADR, peiriant cyflwr ymlaen-yn-unig, LLM lleol, Ollama, OCR, cyfrifiadura ymylol, CSP, penawdau diogelwch, piblinell ddata, peirianneg cost, maniffesto SQLite, D1, R2, KV
**Hypergysylltiadau:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Pam mae ufolens.com wedi'i adeiladu fel y mae

Nodiadau ar y tri phenderfyniad a luniodd [ufolens.com](https://www.ufolens.com) (yr ail-adeiladu chwilioadwy, amlieithog o [archif PURSUE UAP](https://www.war.gov/ufo)). Croeso i sylwadau / gwrthwynebiad.

### 1. Mae'r biblinell yn beiriant cyflwr ymlaen-yn-unig — yn fwriadol

Cyflyrau: `darganfod → lawrlwytho → ocr_wedi'i_wneud → cyfieithu → cyhoeddi`. Dim ond ymlaen mae dogfen yn symud, a dim ond pan fo gwaith i'w wneud. Nid yw cynnwys a gyhoeddir byth yn cael ei ailbrosesu oni bai bod synhwyrydd delta yn gweld bod y ffynhonnell wedi newid mewn gwirionedd.

**Pam:** OCR + cyfieithu yw'r gweithrediadau drud, ac mae'r archif yn tyfu dros amser. Mae gan biblinell sy'n "ail-redeg popeth i fod yn ddiogel" gost ddiderfyn. Mae gwneud trawsnewidiadau yn ôl yn amhosibl yn gwneud bil afreolus yn amhosibl. Mae'r nenfwd cost yn eiddo i'r graff cyflwr, nid i wyliadwriaeth y gweithredwr.

**Cost:** mae mudo sgema ac ailbrosesu'n fwriadol yn fwriadol lletchwith. Cyfaddawd derbyniol.

### 2. Mae OCR a chyfieithu'n rhedeg ar LLM lleol, nid API cwmwl

OCR: peiriant ffynhonnell agored, Tesseract CLI wrth gefn. Cyfieithu + NER: Gemma drwy Ollama, ar liniadur Apple Silicon.

**Pam:** dim cost ymylol fesul dogfen; atgynhyrchadwy (model sefydlog + anogwyr); ac mae'n rhaid i'r cam adalw redeg o IP preswyl beth bynnag (mae'r ffynhonnell y tu ôl i Akamai Bot Manager — mae `curl` yn cael 403), felly mae gliniadur yn y ddolen beth bynnag.

**Cost:** mae ansawdd y cyfieithiad yn is na model arloesol. Ar gyfer corws cyfeirio lle mae'r Saesneg gwreiddiol bob amser un clic i ffwrdd, mae hynny'n iawn. Nid ydym yn honni bod y cyfieithiadau'n awdurdodol.

### 3. Mae'r ddwy hanner yn rhannu un rhyngwyneb yn union: bwndel wedi'i gyhoeddi

Nid yw'r biblinell byth yn ysgrifennu'n uniongyrchol i'r gronfa ddata gynhyrchu. Mae'n allyrru `{ SQL, maniffesto asedau, rhestr glanhau storfa }`. "Cyhoeddi" = cymhwyso'r bwndel hwnnw ymlaen (gwthio SQL i'r DB SQL ymylol, cydamseru asedau i storfa wrthrychau, glanhau'r allweddi storfa a enwir).

**Pam:** gall yr ochr leol a'r ochr ymylol esblygu'n annibynnol; mae'r bwndel yn adolygedig; ac mae gan "ddefnyddio data" yr un siâp bob tro. Mae'r Worker yn ap bach TypeScript/Hono — CSP llym (dim `unsafe-inline`; mae JSON-LD mewnol wedi'i binio â sha256), `Accept-Language` + negodi gwlad→iaith, storfa dudalen KV 30 diwrnod, cron cadw tŷ dyddiol — a does byth angen iddo wybod sut y crëwyd y data.

**Cost:** mae newid i sgema D1 yn cyffwrdd â dwy ffeil (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Yswiriant rhad.

### Pethau na ellir eu negodi sydd wedi'u pobi i'r ymddygiad

- Heb gysylltiad â llywodraeth yr U.D.; dim arwyddluniau swyddogol.
- Mae golygiadau'r ffynhonnell yn cael eu cadw, byth yn cael eu gwrthdroi.
- Fideo wedi'i briodoli i DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ar draws y safle — yn fynegeiadwy gan beiriannau chwilio, wedi'i optio allan o grafu AI.

Yn fyw: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
