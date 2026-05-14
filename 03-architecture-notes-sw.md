# GitHub — Chapisho la 3 kati ya 3 · Maelezo ya usanifu (Majadiliano ya mtindo wa ADR)

**Tumia kama:** Majadiliano chini ya "Onyesha na ueleze" / "Usanifu", au mbegu ya ADR ya `docs/`.
**Maneno muhimu:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Viungo:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kwa nini ufolens.com imejengwa jinsi ilivyo

Maelezo juu ya maamuzi matatu yaliyounda [ufolens.com](https://www.ufolens.com) (ujenzi upya unaoweza kutafutwa, wa lugha nyingi wa [hazina ya PURSUE UAP](https://www.war.gov/ufo)). Maoni / upinzani vinakaribishwa.

### 1. Pipeline ni mfumo wa hali ya usonge mbele pekee — kwa kusudi

Hali: `imegunduliwa → imepakuliwa → ocr_imekamilika → imetafsiriwa → imechapishwa`. Hati husonga mbele tu, na pale tu kuna kazi ya kufanya. Maudhui yaliyochapishwa hayachakatwi tena isipokuwa kigunduzi cha tofauti kimeona chanzo kimebadilika.

**Kwa nini:** OCR + tafsiri ni shughuli za gharama kubwa, na hazina inakua kwa muda. Pipeline ambayo "inaendesha kila kitu upya ili kuwa salama" ina gharama isiyo na kikomo. Kufanya mabadiliko ya kurudi nyuma kuwa yasiyowezekana hufanya bili inayokimbia kuwa isiyowezekana. Dari ya gharama ni sifa ya grafu ya hali, sio ya uangalifu wa mwendeshaji.

**Gharama:** uhamaji wa skema na uchakataji upya wa makusudi ni mgumu kwa makusudi. Mbadilishano unaokubalika.

### 2. OCR na tafsiri huendeshwa kwenye LLM ya ndani, sio API ya wingu

OCR: injini ya chanzo-wazi, mbadala ya Tesseract CLI. Tafsiri + NER: Gemma kupitia Ollama, kwenye kompyuta ndogo ya Apple Silicon.

**Kwa nini:** gharama ndogo ya ziada kwa kila hati; inaweza kurudiwa (mfano maalum + maelekezo); na hatua ya kuchukua tayari inapaswa kuendeshwa kutoka kwa IP ya makazi (chanzo kiko nyuma ya Akamai Bot Manager — `curl` inapata 403), kwa hivyo kompyuta ndogo iko kwenye mzunguko hata hivyo.

**Gharama:** ubora wa tafsiri uko chini ya mfano wa hali ya juu. Kwa hifadhidata ya marejeleo ambapo Kiingereza cha asili kiko umbali wa kubofya mara moja, hiyo ni sawa. Hatudai tafsiri ni rasmi.

### 3. Nusu mbili zinashiriki kiolesura kimoja hasa: kifurushi kilichochapishwa

Pipeline haiandiki kamwe kwenye hifadhidata ya uzalishaji moja kwa moja. Inatoa `{ SQL, maelezo ya mali, orodha ya kusafisha kashe }`. "Kuchapisha" = tumia kifurushi hicho mbele (sukuma SQL kwenye hifadhidata ya SQL ya pembeni, sawazisha mali kwenye hifadhi ya vitu, safisha funguo za kashe zilizotajwa).

**Kwa nini:** upande wa ndani na upande wa pembeni unaweza kubadilika kwa kujitegemea; kifurushi kinaweza kukaguliwa; na "kupeleka data" kuna umbo sawa kila wakati. Worker ni programu ndogo ya TypeScript/Hono — CSP kali (hakuna `unsafe-inline`; inline JSON-LD imebandikwa na sha256), mazungumzo ya `Accept-Language` + nchi → lugha, kashe ya ukurasa ya KV ya siku 30, cron ya usafi wa kila siku — na haihitaji kamwe kujua jinsi data ilivyotengenezwa.

**Gharama:** mabadiliko ya skema ya D1 yanagusa faili mbili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Bima ya bei nafuu.

### Mambo yasiyojadiliwa yaliyojengwa katika tabia

- Haihusiani na serikali ya Marekani; hakuna nembo rasmi.
- Maeneo yaliyofichwa kwenye chanzo huhifadhiwa, hayabadilishwi kamwe.
- Video imehusishwa na DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` kote kwenye tovuti — inaweza kuorodheshwa na injini za utafutaji, imeondolewa kwenye uchunaji wa AI.

Moja kwa Moja: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
