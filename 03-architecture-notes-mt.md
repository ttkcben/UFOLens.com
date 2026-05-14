# GitHub — Post 3 minn 3 · Noti dwar l-arkitettura (Diskussjoni stil ADR)

**Użu bħala:** Diskussjoni taħt "Uri u għid" / "Arkitettura", jew bidu ta' ADR f'`docs/`.
**Kliem ewlieni:** arkitettura, ADR, magna tal-istat li timxi 'l quddiem biss, LLM lokali, Ollama, OCR, edge computing, CSP, headers tas-sigurtà, pipeline tad-dejta, inġinerija tal-ispejjeż, manifest SQLite, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Għaliex ufolens.com huwa mibni kif inhu

Noti dwar it-tliet deċiżjonijiet li ffurmaw [ufolens.com](https://www.ufolens.com) (il-bini mill-ġdid li jista' jitfittex u multilingwi tal-arkivju [PURSUE UAP](https://www.war.gov/ufo)). Kummenti / reazzjonijiet huma milqugħa.

### 1. Il-pipeline huwa magna tal-istat li timxi 'l quddiem biss — apposta

Stati: `discovered → downloaded → ocr_done → translated → published`. Dokument jimxi 'l quddiem biss, u biss meta jkun hemm xogħol xi jsir. Il-kontenut ippubblikat qatt ma jiġi pproċessat mill-ġdid sakemm ditekter tad-delta ma jarax li s-sors fil-fatt inbidel.

**Għaliex:** L-OCR + it-traduzzjoni huma l-operazzjonijiet li jiswew il-flus, u l-arkivju jikber maż-żmien. Pipeline li "jerġa' jħaddem kollox għas-sigurtà" għandu spiża bla limitu. Li tagħmel tranżizzjonijiet b'lura impossibbli tagħmel kont li jikber bla kontroll impossibbli. Il-limitu massimu tal-ispiża huwa proprjetà tal-graff tal-istat, mhux tal-viġilanza tal-operatur.

**Spiża:** il-migrazzjonijiet tal-iskema u l-ipproċessar mill-ġdid apposta huma deliberatament skomdi. Kompromess aċċettabbli.

### 2. L-OCR u t-traduzzjoni jaħdmu fuq LLM lokali, mhux fuq cloud API

OCR: magna open-source, fallback Tesseract CLI. Traduzzjoni + NER: Gemma permezz ta' Ollama, fuq laptop Apple Silicon.

**Għaliex:** spiża marġinali żero għal kull dokument; riproduċibbli (mudell u prompts fissi); u l-pass tal-fetch diġà jrid jaħdem minn IP residenzjali (is-sors jinsab wara Akamai Bot Manager — `curl` jieħu 403), għalhekk laptop huwa involut xorta waħda.

**Spiża:** il-kwalità tat-traduzzjoni hija inqas minn mudell tal-fruntiera. Għal corpus ta' referenza fejn l-Ingliż oriġinali huwa dejjem klikk waħda 'l bogħod, dan huwa tajjeb. Aħna ma nippretendux li t-traduzzjonijiet huma awtorevoli.

### 3. Iż-żewġ nofsijiet jaqsmu eżattament interface wieħed: pakkett ippubblikat

Il-pipeline qatt ma jikteb direttament fid-database tal-produzzjoni. Huwa joħroġ `{ SQL, manifest tal-assi, lista ta' tindif tal-cache }`. "Pubblikazzjoni" = applika dak il-pakkett 'il quddiem (imbotta SQL lejn l-edge SQL DB, issinkronizza l-assi mal-object storage, naddaf iċ-ċwievet tal-cache msemmija).

**Għaliex:** in-naħa lokali u n-naħa edge jistgħu jevolvu b'mod indipendenti; il-pakkett jista' jiġi rivedut; u "skjera d-dejta" għandha l-istess forma kull darba. Il-Worker huwa app żgħira TypeScript/Hono — CSP strett (l-ebda `unsafe-inline`; JSON-LD inline huwa sha256-pinned), negozjar `Accept-Language` + pajjiż→lingwa, cache tal-paġna KV ta' 30 jum, cron ta' manutenzjoni ta' kuljum — u qatt ma jeħtieġ li jkun jaf kif inħolqot id-dejta.

**Spiża:** bidla fl-iskema D1 tmiss żewġ fajls (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Assigurazzjoni rħisa.

### Elementi mhux negozjabbli integrati fl-imġiba

- Mhux affiljat mal-gvern tal-Istati Uniti; l-ebda emblema uffiċjali.
- Ir-redazzjonijiet tas-sors huma ppreservati, qatt ma jitreġġgħu lura.
- Vidjow attribwit lil DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` fuq is-sit kollu — indiċjabbli mit-tiftix, magħżul barra mill-iscraping tal-AI.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

