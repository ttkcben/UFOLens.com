# GitHub — Post 2 de 3 · Gairm do luchd-cuideachaidh / "deagh chiad chùisean"

**Cleachd mar:** Dheasbad prìneach ("A' cur ri & deagh chiad chùisean") no mar ro-ràdh do CONTRIBUTING.md.
**Faclan-luirg:** stòr fosgailte, cur ri, deagh chiad chùis, i18n, ionadailicheadh, OCR, Python, TypeScript, Vitest, pytest, ruigsinneachd, UAP, dàta fosgailte
**Ceanglaichean-lìn:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## A’ cur ri ufolens.com

Bidh [ufolens.com](https://www.ufolens.com) ag atharrachadh [tasglann PURSUE UAP](https://www.war.gov/ufo) Roinn Cogaidh nan S.A. gu àrd-ùrlar so-rannsaichte, ioma-chànanach le [API poblach](https://www.ufolens.com/api/v1). Tha e dà leth — pìob-loidhne ion-ghabhail Python ionadail (`pipeline/`) agus app iomaill TypeScript/Hono (`worker/`) — a’ coinneachadh aig aon eadar-aghaidh: pasgan SQL + maoin foillsichte.

Chan fheum thu teisteanasan sgòth sam bith airson cur ris. Tha modalan bunaiteach na pìob-loidhne mar stdlib-a-mhàin agus bidh deuchainnean an Worker a’ ruith an aghaidh stòradh sa chuimhne.

### Suidheachadh

```bash
# pipeline
python3 -m pytest pipeline/tests/          # bu chòir a h-uile càil a bhith uaine, chan eil feum air stàladh pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Far a bheil cuideachadh as fheumaile

**i18n / ionadailicheadh** — Is e `worker/src/i18n/ui-strings.json` tùs nan sreathan UI. Tha lèirmheas le neach-labhairt dùthchasach air ionadail sam bith nach eil sa Bheurla air leth luachmhor: glac toradh inneil neònach, ceartaich cùisean RTL/cruth, leasaich cùisean iomallach ann an rèiteachadh cànain.

**Càileachd OCR** — ro-phròiseasadh nas fheàrr air seann sganaidhean clò-sgrìobhte mus tèid OCR a dhèanamh; acfhainn measaidh a’ dèanamh coimeas eadar an einnsean stòr-fosgailte agus cùl-taic Tesseract air duilleagan sampaill.

**Ruigsinneachd** — dèan sgrùdadh air na duilleagan a chaidh a thoirt seachad (`worker/src/render/`) an aghaidh WCAG; tha an CSP teann (gun `unsafe-inline`), mar sin feumaidh fuasglaidhean obrachadh taobh a-staigh sin.

**Eòlas-obrach API** — `worker/src/routes/` — duilleagachadh, sìoladh, tuairisgeul OpenAPI, teachdaichean eisimpleir.

**Seasmhachd na pìob-loidhne** — barrachd shlighean ìsleachaidh gràsmhor, aithris adhartais nas fheàrr, cùisean iomallach lorg-delta (`pipeline/lib/delta.py`).

**Sgrìobhainnean** — `docs/20260511/` (繁體中文; is e `00-*` an clàr-amais). Thathas a’ cur fàilte air eadar-theangachadh de na sgrìobhainnean dealbhaidh gu Beurla.

### Riaghailtean bunaiteach

- A h-uile slighe coimeasach — feumaidh am pròiseact a bhith so-ghiùlain thar innealan. Gun shlighean iomlan cruaidh-chòdaichte.
- Na cuir eisimeileachd pip ri modal *cridhe* pìob-loidhne. Faodaidh ìrean roghainneil pacaidean roghainneil a chleachdadh, agus feumaidh iad ìsleachadh gu gràsmhor às an aonais.
- Na lagaich an inneal-stàite a ghluaiseas air adhart a-mhàin — sin am mullach cosgais.
- Na cuir a-steach suaicheantasan oifigeil riaghaltas nan S.A., agus na cuir a-steach dad a chuireas cùl ri deasachaidhean tùsail.
- Bidh atharraichean sgeama D1 a’ beantainn ri **dà** fhaidhle: `pipeline/lib/manifest_schema.sql` agus `db/schema.sql`.
- Deuchainnean le còd ùr. Teachdaireachdan gealltanas-gnàthach.

Leugh `CLAUDE.md` agus `docs/20260511/00-*` an toiseach, an uairsin fosgail cùis gus beachdachadh air rud sam bith structarail mus dèan thu am PR.
