# GitHub — Post 3 jeh 3 · Noteyn Ard-obbrinys (Resooneyaght ayns cummey ADR)

**Ymmyd:** myr Resooneyaght fo "Taishbyn as Insh" / "Ard-obbrinys", ny myr sheel `docs/` ADR.
**Focklyn-ogher:** ard-obbrinys, ADR, jeshaght stayd er oaie-ynrican, LLM ynnydagh, Ollama, OCR, co-earrooaghey oir, CSP, kione-focklyn shickyraght, pib-linney data, jeshaghtaght costys, manifest SQLite, D1, R2, KV
**Hyperchianglaghyn:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cre'n fa ta ufolens.com troggit myr t'eh

Noteyn er ny tree reihyn cummeydagh [ufolens.com](https://www.ufolens.com) (yn aa-hroggal ronsaghey-ys, yl-hengagh jeh tasht-fysseree [PURSUE UAP](https://www.war.gov/ufo)). Ta co-lowaghey / cur noi failtit.

### 1. Ta'n pib-linney ny jeshaght stayd er oaie-ynrican — er y voirey

Staydyn: `feddynit → lhieedynit → ocr_jeant → çhyndaait → soilshit magh`. Cha nel docamad gleashaghey agh er oaie, as cha nel eh gleashaghey agh tra ta obbyr ry-yannoo. Cha nel stoo soilshit magh rieau goll er aa-obbraghey mannagh vel lorgeyder delta fakin dy ren y bun caghlaa dy firrinagh.

**Cre'n fa:** She OCR + çhyndaa-claare ny obbraghyn costyllagh, as ta'n tasht-fysseree gaase harrish traa. Ta costys neuchaglit ec pib-linney ta "aa-roie dagh ooilley nhee son ve shickyr". Liorish jannoo arraghey ergooyl neuyantagh, t'eh jannoo bille neuchaglit neuyantagh. She tro jeh'n ghraf stayd eh mullagh y chostys, cha nee jeh arrey yn obbraidagh.

**Costys:** ta arraghey skeeamey as aa-obbraghey er y voirey neuchooie dy yiooldagh. Co-laueeaght so-ghoaill rish.

### 2. Ta OCR as çhyndaa-claare roie er LLM ynnydagh, cha nee API ooilley

OCR: jeshaght foshlit-foshlit, ergooyl Tesseract CLI. Çhyndaa-claare + NER: Gemma trooid Ollama, er laptop Apple Silicon.

**Cre'n fa:** gyn costys oirragh gagh docamad; aa-ghientynagh (model + praintyn soit); as ta'n keim feddynys hannah lhisagh roie veih IP beaghee (ta'n bun çheu-chooyl jeh Akamai Bot Manager — ta `curl` geddyn 403), myr shen ta laptop 'sy loopagh cooidjioo.

**Costys:** ta keyllid y çhyndaa-claare fo model toshee. Son corpys lioaragh raad ta'n Vaarle vunneydagh rieau un chlic er-mayrn, ta shen mie dy liooar. Cha nel shin gra dy vel ny çhyndaaghyn oikoil.

### 3. Ta'n daa lieh rheynn un eddyr-oaie cruinn: bundeil soilshit magh

Cha nel y pib-linney rieau screeu dys y stoyr-fysseree gientynys dy jeeragh. T'eh cur magh `{ SQL, manifest cooid, rolley glenney-tasht }`. "Soilshaghey magh" = cur y bundeil shen er oaie (cur SQL dys y DB SQL oir, co-chruinnaghey cooid dys stoyral nhee, glenney ny h-oghryn tasht enmyssit).

**Cre'n fa:** foddee yn çheu ynnydagh as yn çheu oir aase dy neuchrogheydagh; ta'n bundeil so-aa-vriwnyssagh; as ta'n cummey cheddin ec "cur data magh" dagh keayrt. She app beg TypeScript/Hono eh y Worker — CSP geyre (gyn `unsafe-inline`; ta JSON-LD 'sy linney sha256-festit), co-chruinnaghey `Accept-Language` + çheer→çhengey, tasht duillag KV 30-laa, cron obbyr-thie laaoil — as cha nel feme erbee echey rieau oayllys ve echey er kys va'n data jeant.

**Costys:** ta caghlaa skeeamey D1 bwoalley daa 'ile (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Jee-insh cheap.

### Neuchaglaaee bun-vun-vunnit 'syn ymmyrkey

- Gyn co-chiangley rish reiltys ny SU; gyn cowraghyn oikoil.
- Ta ardoilghyn bunneydagh freaylt, cha nel ad rieau currit er ash.
- Feyshtyn currit sheese da DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` feie'n ynnyd-eggey — so-ronsaghey-ayrdeksagh, reihit magh ass scraapal AI.

Bio: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
