# GitHub — Okuthunyelwe 3 koku-3 · Amanothi esakhiwo (Ingxoxo yesitayela se-ADR)

**Sebenzisa njenge:** i-Discussion ngaphansi kwe-"Show and tell" / "Architecture", noma imbewu ye-ADR ye-`docs/`.
**Amagama ayisihluthulelo:** isakhiwo, i-ADR, umshini wesimo oya phambili kuphela, i-LLM yendawo, i-Ollama, i-OCR, i-edge computing, i-CSP, izihloko zokuphepha, i-pipeline yedatha, ubunjiniyela bezindleko, imanifesti ye-SQLite, i-D1, i-R2, i-KV
**Ama-hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kungani i-ufolens.com yakhiwe ngendlela eyakhiwe ngayo

Amanothi ezinqumweni ezintathu ezilolonge i-[ufolens.com](https://www.ufolens.com) (ukwakhiwa kabusha okuseshekayo, okunezlimi eziningi kwengobo yomlando ye-[PURSUE UAP archive](https://www.war.gov/ufo)). Amazwana / ukuphikisa kwamukelekile.

### 1. I-pipeline ingumshini wesimo oya phambili kuphela — ngenhloso

Izimo: `kutholakele → kulandiwe → i-ocr_yenziwe → kuhunyushiwe → kushicilelwe`. Idokhumenti ihamba phambili kuphela, futhi kuphela lapho kunomsebenzi okufanele wenziwe. Okuqukethwe okushicilelwe akukaze kuphinde kucutshungulwe ngaphandle kokuthi umtshina we-delta ubone ukuthi umthombo ushintshe ngempela.

**Kungani:** I-OCR + ukuhumusha yimisebenzi ebizayo, futhi ingobo yomlando iyakhula ngokuhamba kwesikhathi. I-pipeline "ephinda isebenze yonke into ukuze iphephe" inezindleko ezingenamkhawulo. Ukwenza izinguquko eziya emuva zibe yinto engenakwenzeka kwenza isikweletu esibalekayo sibe yinto engenakwenzeka. Umkhawulo wezindleko uyisici segrafu yesimo, hhayi sokuqapha komsebenzisi.

**Izindleko:** ukufuduka kwesikimu nokucutshungulwa kabusha ngenhloso kunzima ngamabomu. Ukuyekelela okwamukelekayo.

### 2. I-OCR nokuhumusha kusebenza kwi-LLM yendawo, hhayi kwi-API yamafu

I-OCR: injini yomthombo ovulekile, i-Tesseract CLI isipele. Ukuhumusha + i-NER: i-Gemma nge-Ollama, kwi-laptop ye-Apple Silicon.

**Kungani:** izindleko ezincane eziyiziro zedokhumenti ngayinye; kuyaphindeka (imodeli engaguquki + iziyalezo); futhi isinyathelo sokulanda sesivele kufanele sisebenze kusuka kwi-IP yokuhlala (umthombo ungemuva kwe-Akamai Bot Manager — i-`curl` ithola i-403), ngakho-ke i-laptop isesekethe vele.

**Izindleko:** ikhwalithi yokuhumusha ingaphansi kwemodeli esezingeni eliphezulu. Kwi-corpus eyinkomba lapho isiNgisi sokuqala sihlala sikuchofoza okukodwa kude, lokho kulungile. Asisho ukuthi ukuhumusha kugunyaziwe.

### 3. Izingxenye ezimbili zabelana ngesixhumi esisodwa ngqo: isixha esishicilelwe

I-pipeline ayilokothi ibhale ngqo kusizindalwazi sokukhiqiza. Ikhipha i-{ SQL, imanifesti yempahla, uhlu lokuhlanza i-cache }. "Ukushicilela" = sebenzisa leso sixha phambili (phusha i-SQL kwi-edge SQL DB, vumelanisa izimpahla kusitoreji sento, hlanza okhiye be-cache abaqanjwe amagama).

**Kungani:** uhlangothi lwendawo nohlangothi lwe-edge kungathuthuka ngokuzimela; isixha siyabuyekezwa; futhi "ukufaka idatha" kunomumo ofanayo njalo. I-Worker iwuhlelo lokusebenza oluncane lwe-TypeScript/Hono — i-CSP eqinile (akukho `unsafe-inline`; i-inline JSON-LD i-sha256-pinned), i-`Accept-Language` + ukuxoxisana kwezwe→ulimi, i-cache yekhasi le-KV yezinsuku ezingama-30, i-cron yokuhlanza yansuku zonke — futhi ayidingi ukwazi ukuthi idatha yenziwe kanjani.

**Izindleko:** ushintsho lwesikimu se-D1 luthinta amafayela amabili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Umshwalense oshibhile.

### Okungaxoxiswana ngakho okufakwe ekuziphatheni

- Akuhlangene nohulumeni wase-U.S.; azikho izimpawu ezisemthethweni.
- Ukuhlelwa komthombo kuyalondolozwa, akukaze kuhlehliswe.
- Ividiyo ibizwa nge-DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` kuyo yonke isayithi — iyakwazi ukufakwa kunkomba yokusesha, ikhethe ukuphuma ekuklwebheni kwe-AI.

Isebenza bukhoma: https://www.ufolens.com · I-API: https://www.ufolens.com/api/v1

