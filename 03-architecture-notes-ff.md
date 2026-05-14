# GitHub — Winndannde 3 nder 3 · Teskuyaaji mahdi (Jeewte-jeewte mbaadi ADR)

**Huutoraade no:** Jeewte-jeewte les "Hollude e haalde" / "Mahdi", malla iwdi ADR `docs/`.
**Konnguɗi teeŋtuɗi:** mahdi, ADR, masiŋa statu yahde-to-yeeso, LLM nokkuure, Ollama, OCR, hisnaaki dow-ko-toɓɓe, CSP, headers kisal, pipeline kabaruuji, injiniyaagal coggu, deftere SQLite, D1, R2, KV
**Jokkorli:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ko waɗi ufolens.com mahiraa no o mahiraa

Teskuyaaji dow pellitte tati ɗe mbayli [ufolens.com](https://www.ufolens.com) (waylinde ɗaɓɓitotoonde, ɗemɗe keewɗe nde [defterdu PURSUE UAP](https://www.war.gov/ufo)). Miijooji / luural ina njaɓɓaama.

### 1. Pipeline ko masiŋa statu yahde-to-yeeso — e anniya

Statuuji: `yiitaama → jippinaama → ocr_timmi → firtaama → yaltinaama`. Binndol ina yaha tan to yeeso, e tan so golle ina woodi. Ko yaltinaa meeɗaa wayleede so wonaa si yiytorde delta yii waylitaare tigi-rigi e iwdi.

**Ko waɗi:** OCR + firugol ko golle coggu mawngo, e defterdu nduu ina mawnude e wakkati. Pipeline "gollitooɗo fuu ngam hoolaare" ina jogii coggu mo alaa keerol. Waɗde waylitaare to caggal ko huunde nde waawaa waɗde, waɗi faktu mawɗo waawaa waɗde. Keerol coggu ko jikku graph statu, wonaa hakkilantaagal gollinoowo.

**Coggu:** waylitaare schema e waylude-e-anniya ina tiiɗi e anniya. Ko yeeyre jaɓaande.

### 2. OCR e firugol ina ngolla e LLM nokkuure, wonaa API cloud

OCR: masiŋa open-source, Tesseract CLI walla. Firugol + NER: Gemma rewrude e Ollama, dow laptop Apple Silicon.

**Ko waɗi:** coggu marginal zero ngam kala binndol; ina waawi wayleede (model + prompts tabitiiɗi); e step heɓtude foti gollude diga IP hoɗorde (iwdi ndii ina woni caggal Akamai Bot Manager — `curl` ina heɓa 403), so laptop ina woodi e nder mum.

**Coggu:** moƴƴere firugol ina les model keerol. Ngam defterdu referans ɗo Engele asliijo ina heɓoo e klik gooto, ɗuum ina moƴƴi. Min mbiydaaki firugol ngol ko laamuyankeewol.

### 3. Geɓe ɗiɗi ɗee ina kawra e interface gooto tan: go'o yaltinaaɗo

Pipeline meeɗaa winndude to database production fota-fota. O yaltina `{ SQL, deftere jawdi, limngal ittude-cache }`. "Yaltinde" = huutoraade go'o ngoo to yeeso (dudda SQL to DB SQL dow-ko-toɓɓe, sync jawdi to resrude, ittude coktirɗe cache inniraaɗe).

**Ko waɗi:** bannge nokkuure e bannge dow-ko-toɓɓe ina mbaawi ɓamtude ceertuɗi; go'o ngoo ina waawi ƴeewteede; e "deploy data" ina jogii mbaadi ngootiri kala wakkati. Worker ko app TypeScript/Hono famaro — CSP tiiɗɗo (alaa `unsafe-inline`; inline JSON-LD ko sha256-pinned), `Accept-Language` + yeewtere leydi→ɗemngal, cache kelle KV balɗe 30, cron laɓɓinirɗo kala nyalawma — e meeɗaa haajannde anndude no kabaruuji ɗii mbaɗiraa.

**Coggu:** waylitaare schema D1 ina mema files ɗiɗi (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Ko hoolaare coggu famaro.

### Ko waawaa yeewteede ko naatnaa e jikku

- Hawtaaki e laamu Aameerik; alaa alaama laamu.
- Suuɗngo iwdi ina reenee, meeɗaa wayleede.
- Wideyooji ina njeyaa e DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` dow lowre fuu — ina waawi wonde index ɗaɓɓitorde, woppii scraping AI.

To woodi: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
