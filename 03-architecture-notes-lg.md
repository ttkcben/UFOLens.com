# GitHub — Ekiwandiko 3 kya 3 · Ebiwandiko by'enzimba (Okunyumya kw'ekika kya ADR)

**Kozesa nga:** Okunyumya wansi w'"Okulaga n'okubuulira" / "Enzimba", oba `docs/` ensigo ya ADR.
**Ebigambo ebikulu:** enzimba, ADR, enkola y'okukola ekiddako yokka, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Enkolagana:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Lwaki ufolens.com yazimbibwa bw'etyo

Ebiwandiko ku nsalessale essatu ezaateekawo [ufolens.com](https://www.ufolens.com) (okuddamu okuzimba okw'ennimi nnyingi, okusobola okunoonyezebwa okw'[enkumu ya PURSUE UAP](https://www.war.gov/ufo)). Okulanga / okuwakanya kwanirizibwa.

### 1. Pipeline ye nkola y'okukola ekiddako yokka — kya bugenderere

Embeera: `discovered → downloaded → ocr_done → translated → published`. Ekiwandiiko kitambula mu maaso kyokka, era nga waliyo omulimu gw'okukola. Ebitongozeddwa tebiddamu kukolebwako okuggyako nga akatundu akalaba enkyukakyuka kalaba nti ensibuko yakyuka.

**Lwaki:** OCR + okuvvuunula bye bikolwa eby'omuwendo, era enkumu ekula n'ebiseera. Pipeline "eddamu okukola byonna okuba omukakafu" erina omuwendo ogutakoma. Okufuula okudda emabega nga tekisoboka kifuula omuwendo ogusususe okuba ogutasoboka. Ekomo ly'omuwendo kintu kya giraafu y'embeera, so si kya kulondoola kwa mukozesa.

**Omuwendo:** okukyusa enkola n'okuddamu okukola okw'obugenderere byakolebwa nga bizibu. Enkyukakyuka enzikirizibwa.

### 2. OCR n'okuvvuunula bikolera ku local LLM, so si ku cloud API

OCR: enjini ya open-source, Tesseract CLI fallback. Okuvvuunula + NER: Gemma nga eyita mu Ollama, ku kompyuta ya Apple Silicon.

**Lwaki:** tewali muwendo gwonna ku buli kiwandiiko; kisobola okuddamu okukolebwa (enkola entongole + ebigambo); era ekitundu ky'okufuna kirina okukolera ku IP y'ewaka (ensibuko eri emabega wa Akamai Bot Manager — `curl` efuna 403), n'olwekyo kompyuta y'ewaka erina okuba mu nteekateeka.

**Omuwendo:** omutindo gw'okuvvuunula guli wansi w'enkola ey'oku ntikko. Eri enkumu y'okulabirako awali Olungereza olw'olubereberye olusobola okufunika mu kaseera katono, ekyo kirungi. Tetugamba nti okuvvuunula kulina obuyinza.

### 3. Ebitundu bibiri bigabana ekintu kimu kyokka: omuganda ogutongozeddwa

Pipeline tewandiika mu database y'omukutu butereevu. Efulumya `{ SQL, asset manifest, cache-purge list }`. "Okutongoza" = okuteeka omuganda ogwo mu nkola (okuteeka SQL mu edge SQL DB, okugatta assets ku terekero ly'ebintu, okuggyawo ebikunci bya cache ebimanyiddwa).

**Lwaki:** oludda olw'ewaka n'oludda olw'oku nsalo bisobola okukula mu ngeri ey'enjawulo; omuganda gusobola okwekenneenyezebwa; era "oku deploy data" kulina enkola y'emu buli kiseera. Worker ye appu ya TypeScript/Hono entono — CSP nkakali (tewali `unsafe-inline`; inline JSON-LD esimbiddwa ku sha256), `Accept-Language` + okutegeeragana kw'ensi→olulimi, KV cache y'emiko y'ennaku 30, n'okulongoosa okwa buli lunaku — era tewetaaga kumanya ngeri data gye yakolebwamu.

**Omuwendo:** enkyukakyuka mu D1 schema ekwata ku fayiro bbiri (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insuwa ya buseere.

### Ebitasobola kukyusibwa ebiteekeddwamu mu nkola

- Si nkolagana ne gavumenti ya U.S.; tewali birango bitongole.
- Ebyagibwamu mu nsibuko bikuumibwa, tebisattululwa.
- Obutambi buvunanyizibwa ku DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ku mukutu gwonna — enjini z'okunoonya zisobola okugiyingira, tegisobola kukozesebwa mu kukwata data ya AI.

Luli ku: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

