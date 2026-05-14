# GitHub — Post 3 of 3 · Zvinonyorwa zveArchitecture (Kurukura kwemutsevo weADR)

**Shandisa se:** Kurukura pasi pe "Show and tell" / "Architecture", kana mbeu ye `docs/` ADR.
**Keywords:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kuti nei ufolens.com yakagadzirwa nenzira ino

Zvinonyorwa pamusoro pezvisarudzo zvitatu zvakagadzira [ufolens.com](https://www.ufolens.com) (kugadzira zvakare kunogona kutsvagwa uye kunotaura mitauro yakawanda kwe [PURSUE UAP archive](https://www.war.gov/ufo)). Maonero kana kuramba kwakugashira.

### 1. Pipeline iyi i-forward-only state machine — zvine chinangwa

Zvimbarara (States): `discovered → downloaded → ocr_done → translated → published`. Document inofamba mberi chete, uye chete kana pane basa rekuita. Zvinonyorwa zvakabhurutsirwa hazvifaniri kugadzirwa zvakare kunze kwekuti delta detector ioone kuti source yashanduka.

**Kuti nei:** OCR + kududzira (translation) ndizvo zviito zvinodhura, uye archive inokura nenguva. Pipeline inoti "dzokorora zvese kuti uve safe" ine mutengo usina miganhu. Kuita kuti kudzokera kumashure kusazvikwanise kunoita kuti bill isakwirire zvisingatarisirwi. Mwero wemutengo (cost ceiling) ichimiro che-state graph, kwete kuchenjera kweoperator.

**Mutengo:** schema migrations nekugadzira zvakare zvine chinangwa zvakagadzirwa kuti zvive zvakaoma. Kutsiura uku kunogamuchirika.

### 2. OCR nekududzira zvinoshanda pa-local LLM, kwete pa-cloud API

OCR: engine ye-open-source, Tesseract CLI fallback. Kududzira + NER: Gemma kuburikidza ne Ollama, pa-laptop ye Apple Silicon.

**Kuti nei:** mutengo wezero paka-document imwe neimwe; inogona kudzokororwa (fixed model + prompts); uye danho re-fetch rinofanira kutanga rishande kubva pa-residential IP (source iri kuseri kwe Akamai Bot Manager — `curl` inowana 403), saka laptop iri mukurukura zvine chikonzero.

**Mutengo:** hunyanzvi hwekududzira huzvisi hwekupfuura frontier model. Kune corpus yereference apo chirungu chakapambiri chete, izvi zvakanaka. Hatitauri kuti kududzira uku ndiko kwakakwana.

### 3. Zvikamu zviviri zvinogovana interface imwe chete: bundle yakabhurutsirwa

Pipeline hainyori pa-production database zvakananga. Inobudisa `{ SQL, asset manifest, cache-purge list }`. "Publishing" = kushandisa bundle iyoyo mberi (kushandisa SQL kuenda ku-edge SQL DB, ku-sync assets kuenda ku-object storage, kuchenesa cache keys dzakazvitsvaga).

**Kuti nei:** chikamu chelocal neche-edge zvinogona kugadzirwa zvakazvimirira; bundle inogona kuongororwa; uye "deploy data" rine chimiro chimwe chete nguva dzose. Worker i-app diki ye TypeScript/Hono — yakasimba CSP (hapana `unsafe-inline`; inline JSON-LD yakagadzirwa se sha256-pinned), `Accept-Language` + kugadzirisana kwenyika→mutauro, 30-day KV page cache, cron ye-housekeeping yezuva nezuva — uye haifaniri kuziva kuti data rakagadzirwa sei.

**Mutengo:** kushanduka kwe-D1 schema kunobata mafayira maviri (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurance isingadhure.

### Zvisingagoni kukurukurwa zvakagadzirwa mukushanda

- Haisingafambidzani nehurumende yeU.S.; hapana zviratidzo zvinozvishumira.
- Kudzima kwezvimwe muzvinyorwa (source redactions) kunochengetedzwa, hakudzokorwi.
- Video inopedzwa kuna DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` site-wide — inogona kutsvagwa (search-indexable), asi AI-scrape-opted-out.

Yave kushanda (Live): https://www.ufolens.com · API: https://www.ufolens.com/api/v1