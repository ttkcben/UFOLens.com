# GitHub — Indatshana 3 kwezi-3 · Amanothi wemithethokhandla (Ingxoxo ye-ADR-style)

**Sebenzisa njenge-:** Ingxoxo ngaphasi kwe-"Khombisa begodu utjele" / "Imithethokhandla", namkha i-`docs/` ADR seed.
**Amagamaqangi:** imithethokhandla, i-ADR, umshini we-state oya phambili kwaphela, i-local LLM, i-Ollama, i-OCR, i-edge computing, i-CSP, ama-header wokuphepha, i-data pipeline, i-cost engineering, i-SQLite manifest, i-D1, i-R2, i-KV
**Izixhumanisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kubayini i-ufolens.com yakhelwe ngendlela eyakhelwe ngayo

Amanothi ngesigaba esithathu esakha i-[ufolens.com](https://www.ufolens.com) (ukwakhiwa kabutjha okungahlolisiseka, okuneelimi ezinengi kwe-[PURSUE UAP archive](https://www.war.gov/ufo)). Amazizo / ukubuyiselwa kwamukelekile.

### 1. I-pipeline ngumshini we-state oya phambili kwaphela — ngamabomu

I-States: `otholiwe → olandiwe → i-ocr yenziwe → ihunyushiwe → iphatlaladisiwe`. Idokhumente iya phambili kwaphela, begodu kuphela nakunomsebenzi okufanele wenziwe. Okuveziweko akukaze kuphinde kulungiswe ngaphandle kobana i-delta detector ibona bona umthombo watjhuguluka.

**Kubayini:** I-OCR + ukuhumusha kuyimisebenzi edulako, begodu i-archive iyakhula ngokuhamba kwesikhathi. I-pipeline ethi "iphinda isebenzise koke ukuthi iphephile" inezindleko ezinganakwa. Ukwenza iintjhijilelo zemuva zingenzeki kwenza i-bill engakhulumeki ingenzeki. Umkhawulo wendleko yisisekelo se-state graph, ingasi ukuphatha ngokuqaphela.

**Indleko:** i-schema migrations kunye nokulungisa kabutjha ngamabomu kukhangeleleki. Ukulinganisa okwamukelekileko.

### 2. I-OCR nokuhumusha kusebenza nge-LLM yangekhaya, ingasi nge-cloud API

I-OCR: injini ye-open-source, i-Tesseract CLI fallback. Ukuhumusha + NER: i-Gemma nge-Ollama, ku-laptop ye-Apple Silicon.

**Kubayini:** akukho ndleko ehlanganisiweko ngedokhumente ngayinye; iyakghoneka (imodel elungisiweko + ama-prompt); begodu isigaba se-fetch kufanele sisebenze kusukela ku-IP yangekhaya (umthombo ungasemuva kwe-Akamai Bot Manager — `curl` ithola i-403), ngalokho i-laptop ihlanganiswa nomsebenzi.

**Indleko:** ikhwalithi yokuhumusha ingaphasi kwemodel eqakathekileko. Kumtapo wokubhekisela lapho isiNgisi sokuqala sihlala sitholakala ngokutjhupha okukodwa, lokho kuhle. Asitjho bona ukuhumusha kunesigunyazo.

### 3. Izigaba ezimbili zabelana ngesikhungo esisodwa kwaphela: i-bundle esiveziweko

I-pipeline ayikali nge-database yokukhiqiza ngokuqondileko. Ikhupha `{ SQL, i-asset manifest, i-cache-purge list }`. "Ukuveza" = sebenzisa i-bundle leyo phambili (faka i-SQL ku-edge SQL DB, vumelanisa ama-assets ku-object storage, sula ama-cache keys abiziweko).

**Kubayini:** ihlangothi lendawo kunye nehlangothi le-edge zingathuthuka ngokuzihlukanisa; i-bundle iyahlolisiseka; begodu "i-deploy data" yi-shape efanako ngaso soke isikhathi. I-Worker yi-TypeScript/Hono app encani — i-strict CSP (akukho `unsafe-inline`; i-inline JSON-LD ishaywa nge-sha256), i-`Accept-Language` + ukukhulumisana ngelizwe→ilimi, i-cache ye-KV yelikhasi yamasuku ama-30, i-cron yokuhlanza yansuku zoke — begodu ayikaseidinge ukwazi bona idatha yenziwa njani.

**Indleko:** intjhijilelo ye-D1 schema ithinta amafayela amabili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Umtjhapharari ongabizi.

### Okungatjhugulukiko okufakwe ebulungeni

- Ayihlangani norhulumende we-U.S.; ayinazo iimbotjhisi ezisemthethweni.
- Ukungenelela komthombo kulondoloziwe, akukaze kunqanyulwe.
- Ividiyo itjho bona ivela ku-DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` kuwo woke umtjhapharari — iyahlolisiseka ngeenjini zokuhlola, asifakiwe ekukhiqizeni nge-AI.

Bukhoma: https://www.ufolens.com · I-API: https://www.ufolens.com/api/v1

