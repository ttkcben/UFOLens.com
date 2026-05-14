# GitHub — Post 3 de 3 · Notaichean ailtireachd (Deasbad stoidhle ADR)

**Cleachd mar:** Dheasbad fo "Seall agus innis" / "Ailtireachd", no mar shìol ADR ann an `docs/`.
**Faclan-luirg:** ailtireachd, ADR, inneal-stàite a ghluaiseas air adhart a-mhàin, LLM ionadail, Ollama, OCR, coimpiutaireachd iomaill, CSP, bannan-cinn tèarainteachd, pìob-loidhne dàta, innleadaireachd chosgaisean, foillseachadh SQLite, D1, R2, KV
**Ceanglaichean-lìn:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Carson a tha ufolens.com air a thogail mar a tha e

Notaichean air na trì co-dhùnaidhean a dhealbh [ufolens.com](https://www.ufolens.com) (an ath-thogail so-rannsaichte, ioma-chànanach de [thasglann PURSUE UAP](https://www.war.gov/ufo)). Thathas a’ cur fàilte air beachdan / eas-aonta.

### 1. Is e inneal-stàite a ghluaiseas air adhart a-mhàin a th’ anns a’ phìob-loidhne — a dh’aona ghnothach

Stàitean: `discovered → downloaded → ocr_done → translated → published`. Cha ghluaiseas sgrìobhainn ach air adhart, agus dìreach nuair a tha obair ri dhèanamh. Cha tèid susbaint fhoillsichte ath-phròiseasadh gu bràth mura faic lorgaire delta gun do dh’atharraich an tùs fhèin.

**Carson:** Is e OCR + eadar-theangachadh na h-obraichean cosgail, agus bidh an tasglann a’ fàs thar ùine. Tha cosgais gun chrìoch aig pìob-loidhne a bhios “ag ath-ruith a h-uile càil gus a bhith sàbhailte”. Le bhith a’ dèanamh eadar-ghluasadan air ais do-dhèanta, bidh bile ruith-air-falbh do-dhèanta. Tha am mullach cosgais na sheilbh aig graf na stàite, chan ann aig furachas a’ ghnìomhaiche.

**Cosgais:** tha imrichean sgeama agus ath-phròiseasadh a dh’aona ghnothach gu math an-fhoiseil. Malairt-off iomchaidh.

### 2. Bidh OCR agus eadar-theangachadh a’ ruith air LLM ionadail, chan e API sgòth

OCR: einnsean stòr-fosgailte, cùl-taic Tesseract CLI. Eadar-theangachadh + NER: Gemma tro Ollama, air laptop Apple Silicon.

**Carson:** cosgais iomaill neoni gach sgrìobhainn; ath-riochdachadh (modail + brosnachaidhean stèidhichte); agus feumaidh an ceum ‘fetch’ ruith bho IP còmhnaidheach co-dhiù (tha an tùs air cùl Akamai Bot Manager — gheibh `curl` 403), mar sin tha laptop san lùb co-dhiù.

**Cosgais:** tha càileachd an eadar-theangachaidh nas ìsle na modail crìche. Airson corpus fiosrachaidh far a bheil a’ Bheurla thùsail an-còmhnaidh aon bhriogadh air falbh, tha sin ceart gu leòr. Chan eil sinn ag agairt gu bheil na h-eadar-theangachaidhean ùghdarrasach.

### 3. Bidh an dà leth a’ co-roinn dìreach aon eadar-aghaidh: pasgan foillsichte

Cha sgrìobh a’ phìob-loidhne gu stòr-dàta an riochdachaidh gu dìreach gu bràth. Bidh i a’ sgaoileadh `{ SQL, foillseachadh maoin, liosta glanadh tasgadan }`. Tha “Foillseachadh” = cuir am pasgan sin air adhart (put SQL chun stòr-dàta SQL iomaill, sioncronaich maoin gu stòradh nithean, glan na h-iuchraichean tasgadan ainmichte).

**Carson:** faodaidh an taobh ionadail agus an taobh iomaill atharrachadh gu neo-eisimeileach; faodar am pasgan ath-sgrùdadh; agus tha “sgaoileadh dàta” den aon chumadh gach turas. Is e app beag TypeScript/Hono a th’ anns an Worker — CSP teann (gun `unsafe-inline`; tha JSON-LD in-loidhne air a phrìneachadh le sha256), `Accept-Language` + rèiteachadh dùthaich→cànain, tasgadan duilleag KV 30-latha, cron glanadh-taighe làitheil — agus cha leig e a leas a bhith eòlach air ciamar a chaidh an dàta a chruthachadh.

**Cosgais:** bidh atharrachadh sgeama D1 a’ beantainn ri dà fhaidhle (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Àrachas saor.

### Neo-rèiteachaidhean air am bèicearachd a-steach don ghiùlan

- Gun cheangal ri riaghaltas nan S.A.; gun suaicheantasan oifigeil.
- Tha deasachaidhean tùsail air an gleidheadh, cha tèid an cur air ais gu bràth.
- Bhidio air a thoirt do DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` air feadh na làraich — so-chlàr-amais le einnseanan luirg, roghnaichte a-mach à sgrìobadh AI.

Beò: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
