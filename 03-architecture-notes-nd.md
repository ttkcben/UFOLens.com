# GitHub — Post 3 of 3 · Amanothi e-Architecture (i-Discussion ye-ADR-style)

**Sebenzisa njenge:** i-Discussion ngaphansi kwe-"Show and tell" / "Architecture", kumbe `docs/` i-ADR seed.
**Amagama angukhiye:** i-architecture, ADR, umshini we-state oya phambili kuphela, i-local LLM, Ollama, OCR, i-edge computing, CSP, izinhloko zokuphepha, i-data pipeline, i-cost engineering, i-SQLite manifest, D1, R2, KV
**Izixhumanisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kungani i-ufolens.com yakhelwe ngendlela eyakhelwe ngayo

Amanothi ngezinqumo ezintathu ezakhe [ufolens.com](https://www.ufolens.com) (ukwakhiwa kabusha okuseshekayo, okunezilimi eziningi kwe-[PURSUE UAP archive](https://www.war.gov/ufo)). Amazwana / ukungavumelani kwamukelekile.

### 1. I-pipeline ingumshini we-state oya phambili kuphela — ngenhloso

Izimo: `discovered → downloaded → ocr_done → translated → published`. Idokhumende iya phambili kuphela, futhi kuphela uma kunomsebenzi okumele wenziwe. Okuqukethwe okushicilelwe akuphindi kucutshungulwe ngaphandle uma i-delta detector ibona ukuthi umthombo ushintshile ngempela.

**Kungani:** I-OCR + ukuhumusha yimisebenzi ebiza kakhulu, futhi i-archive ikhula ngokuhamba kwesikhathi. I-pipeline "ephinda isebenzise konke ukuze iphephe" inezindleko ezinganqunyelwe. Ukwenza ukuguqulwa okuya emuva kungenzeki kwenza isikweletu esingalawuleki singenzeki. Isilinganiso sezindleko siyisakhiwo se-state graph, hhayi sokuqapha komsebenzisi.

**Izindleko:** ukufuduka kweskimu kanye nokuphinda kucutshungulwe ngenhloso kungokungahleliwe ngamabomu. Ukuxazulula okwamukelekayo.

### 2. I-OCR nokuhumusha kwenziwa ku-local LLM, hhayi i-cloud API

I-OCR: injini ye-open-source, i-Tesseract CLI fallback. Ukuhumusha + NER: Gemma nge-Ollama, ku-laptop ye-Apple Silicon.

**Kungani:** izindleko ezinguziro ezingaphezu kwalokho ngedokhumende ngayinye; ziphindaphindeka (imodeli elungisiwe + izicelo); futhi isigaba sokuthola kumele sisebenze kusuka ku-IP yasekhaya (umthombo ungemuva kwe-Akamai Bot Manager — `curl` ithola i-403), ngakho i-laptop isesimweni noma kunjalo.

**Izindleko:** ikhwalithi yokuhumusha ingaphansi kwe-frontier model. Nge-corpus yesithenjwa lapho isiNgisi sokuqala sihlale sikude ngokuchofoza okukodwa, thina singakuzwa. Asisho ukuthi ukuhumusha kunegunya.

### 3. Izingxenye ezimbili zabelana ngesibonisi esisodwa nje: i-bundle eshicilelwe

I-pipeline ayikaze ibhale ngqo ku-database yokukhiqiza. Ikhipha `{ SQL, asset manifest, cache-purge list }`. "Ukushicilela" = sebenzisa leyo bundle phambili (push SQL ku-edge SQL DB, vumelanisa i-assets ku-object storage, susa ama-cache keys anegama).

**Kungani:** uhlangothi lwasendaweni nohlangothi lwe-edge lungathuthuka ngokuzimela; i-bundle ingabuyekezwa; futhi "deploy data" inesimo esifanayo ngaso sonke isikhathi. I-Worker iyi-TypeScript/Hono app encane — i-CSP eqinile (akukho `unsafe-inline`; i-inline JSON-LD yi-sha256-pinned), `Accept-Language` + ukuxoxisana kwezwe→ulwimi, i-cache yepheji ye-KV yezinsuku ezingu-30, i-cron yokuhlanza yansuku zonke — futhi ayikaze idinge ukwazi ukuthi idatha yenziwa kanjani.

**Izindleko:** ushintsho lweskimu se-D1 luthinta amafayela amabili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Umshwalense othengisekayo.

### Okungaxoxisani ngakho okubhekwe ekuziphatheni

- Ayihlangene nohulumeni wase-U.S.; akukho uphawu olusemthethweni.
- Ukuhlelwa komthombo kugcinwa, akukaze kuhlehliswe.
- Ividiyo inikezwa ku-DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` kuwo wonke isayithi — liyasesheka ngezinjini zokusesha, likhishiwe ekuqeqeshweni kwe-AI-scrape.

Ibukhoma: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
