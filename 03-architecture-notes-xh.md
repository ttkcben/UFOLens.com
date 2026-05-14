# GitHub — IsiKhankanya 3 kwezi-3 · Amanqaku oyilo (i-Discussion ye-ADR-style)

**Sisebenzise njenge:** i-Discussion phantsi kwe "Bonisa kwaye uxele" / "Uyilo", okanye imbewu ye-ADR ye `docs/`.
**Amagama aphambili:** uyilo, ADR, umshini welizwe olulodwa lokuya phambili, i-local LLM, Ollama, OCR, i-edge computing, CSP, izihloko zokhuseleko, i-data pipeline, ubunjineli beendleko, i-SQLite manifest, D1, R2, KV
**Iziqhagamshelanisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kutheni i-ufolens.com yakhiwe ngolu hlobo

Amanqaku ngezigqibo ezithathu ezakhe [ufolens.com](https://www.ufolens.com) (ukwakhiwa kwakhona okukhangelweyo, okweenkcubeko ezininzi kwengobo yovimba ye [PURSUE UAP archive](https://www.war.gov/ufo)). Izimvo / ukuchasana kwamkelekile.

### 1. I-pipeline ngumshini welizwe olulodwa lokuya phambili — ngenjongo

Amazwe: `discovered → downloaded → ocr_done → translated → published`. Uxwebhu luya phambili kuphela, kwaye kuphela xa kukho umsebenzi ekufuneka wenziwe. Umxholo opapashiweyo awusoze uphinde wenziwe ngaphandle kokuba isixhobo sokubona idelta sibona ukuba umthombo utshintshile.

**Kutheni:** I-OCR + inguqulelo yimisebenzi ebiza kakhulu, kwaye ingobo yovimba iyakhula ngokuhamba kwexesha. I-pipeline "ephinda isebenze yonke into ukuze ikhuseleke" ineenkozo ezingenakulinganiswa. Ukwenza utshintsho olubuyela umva lungenakwenzeka kwenza intlawulo engalawulekiyo ingenakwenzeka. Isilingi seendleko yipropati yegrafu yelizwe, kungekhona ukuphaphama komsebenzisi.

**Iindleko:** ukufuduswa kwesikimu kunye nokuphinda kuqhubekeke ngenjongo ziinqabileyo ngabom. Ukutshintshiselana okwamkelekileyo.

### 2. I-OCR nenguqulelo zisebenza kwi-LLM yasekhaya, kungekhona i-cloud API

I-OCR: injini ye-open-source, i-Tesseract CLI fallback. Inguqulelo + NER: Gemma nge-Ollama, kwi-laptop ye-Apple Silicon.

**Kutheni:** zero iindleko ezongezelelweyo ngoxwebhu ngalunye; iyaphinda iveliswe (imodel engaguqukiyo + izikhawulezisi); kwaye inyathelo lokufaka idatha sele kufuneka lisebenze kwi-IP yasekhaya (umthombo ungasemva kwe-Akamai Bot Manager — `curl` ifumana i-403), ngoko i-laptop ikwinkqubo nangona kunjalo.

**Iindleko:** umgangatho wenguqulelo ungaphantsi kwemodel yomda. Kwincwadi yesalathiso apho isiNgesi sokuqala sisoloko sikhona ngokucofa kube kanye, oko kulungile. Asibangi ukuba iinguqulelo zinika igunya.

### 3. Izixekana ezibini zabelana nge-interface enye kuphela: ibundle epapashiweyo

I-pipeline ayisoze ibhale kwi-database yokuvelisa ngqo. Ikhupha `{ SQL, asset manifest, cache-purge list }`. "Ukupapasha" = sebenzisa loo bundle phambili (tyhala i-SQL kwi-edge SQL DB, vumelanisa izinto kwindawo yokugcina izinto, coce iindawo zokugcina izinto ezigama).

**Kutheni:** icala lasekhaya kunye necala elisecaleni linokukhula ngokuzimeleyo; ibundle inokuhlolwa; kwaye "idatha yokuthunyelwa" inemilo efanayo ngalo lonke ixesha. I-Worker yi-TypeScript/Hono app encinci — i-CSP ehlaliweyo (akukho `unsafe-inline`; i-inline JSON-LD yi-sha256-pinned), `Accept-Language` + ukuthethathethana ngelizwe→ulwimi, i-cache ye-KV yeentsuku ezingama-30, i-cron yokucoceka kwansuku zonke — kwaye ayisoze ifune ukwazi ukuba idata yenziwe njani.

**Iindleko:** utshintsho lwesikimu se-D1 luchaphazela iifayile ezimbini (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Inshorensi ebiza kancinci.

### Izinto ezingenakutshintshwa ezifakwe kwindlela yokuziphatha

- Ayinxulumananga norhulumente wase-U.S.; akukho mpepho esemthethweni.
- Izikhwebula zomthombo zigciniwe, azisoze zibuyiselwe.
- Ifilimu yevidiyo ibangelwa yi-DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` kwisiza sonke — inokukhangelwa ziinjini zokukhangela, isuswe kuqeqesho lwe-AI.

Phila: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
