# GitHub — Post 3 of 3 · Makanisi na ntina ya botongi (Discussion ya style ya ADR)

**Salelá lokola:** Discussion na nse ya "Lakisa mpe yebisa" / "Architecture", to mbuma ya ADR ya `docs/`.
**Maloba ya ntina:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Ba hyperliens:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mpo na nini ufolens.com etongami ndenge ezali

Makanisi na ntina ya mikano misato oyo epesi lolenge na [ufolens.com](https://www.ufolens.com) (libongoli ya bolukiluki, ya minoko mingi ya [archive ya PURSUE UAP](https://www.war.gov/ufo)). Bakanisi / botɛmɔli biyambi.

### 1. Pipeline ezali machine ya état oyo ekendeke kaka liboso — na nko

Ba états: `discovered → downloaded → ocr_done → translated → published`. Mokanda ekendeke kaka liboso, mpe kaka soki mosala ezali. Contenu oyo ebimisami ezongelamaka ata mokolo moko te longola se soki eloko oyo emonaka mbongwana emoni ete source ebongwani mpenza.

**Mpo na nini:** OCR + libongoli ezali misala ya ntalo, mpe archive ekolaka na ntango. Pipeline oyo "ezongelaka biloko nyonso mpo na kozala na kimia" ezali na ntalo oyo ezangi nsuka. Kopekisa kozonga sima ekómisi facture monene likambo ya mpasi. Ndelo ya ntalo ezali likambo ya graphe ya état, kasi ya bokebi ya mosali te.

**Ntalo:** ba migrations ya schema mpe kozongela na nko ezali mpasi na nko. Mbongwana oyo ekoki kondimama.

### 2. OCR mpe libongoli esalemaka na LLM ya esika moko, kasi na API ya cloud te

OCR: moteur ya open-source, fallback ya Tesseract CLI. Libongoli + NER: Gemma na nzela ya Ollama, na laptop ya Apple Silicon.

**Mpo na nini:** ntalo ya kobakisa ya zero mpo na mokanda moko na moko; ekoki kozongelama (modèle mpe ba prompts oyo etikalaka); mpe eteni ya kozwa esengeli esalama na IP ya ndako (source ezali na sima ya Akamai Bot Manager — `curl` ezwaka 403), yango wana laptop ezali na kati ya likambo.

**Ntalo:** bolembu ya libongoli ezali na nse ya modèle ya liboso. Mpo na corpus ya référence esika Anglais ya ebandeli ezali ntango nyonso na clic moko, yango ezali malamu. Tolobi te ete mabongoli yango ezali ya solo.

### 3. Biteni mibale bikabolaka kaka interface moko: liboke oyo ebimisami

Pipeline ekomaka ata mokolo moko te na base de données ya production mbala moko. Ebimisaka `{ SQL, manifest ya biloko, liste ya bopetoli ya cache }`. "Kobimisa" = kosalela liboke wana liboso (kotinda SQL na DB ya SQL ya pembeni, kosangisa biloko na esika ya kobomba biloko, kopetola ba fungola ya cache oyo etangami).

**Mpo na nini:** eteni ya esika moko mpe eteni ya pembeni bikoki kokola na bonsomi; liboke ekoki kotalelama; mpe "kotia ba données" ezali na lolenge moko ntango nyonso. Worker ezali application moke ya TypeScript/Hono — CSP ya makasi (kozanga `unsafe-inline`; inline JSON-LD ezali sha256-pinned), `Accept-Language` + boyokani ya mboka→monoko, cache ya lokasa ya KV ya mikolo 30, cron ya misala ya mokolo na mokolo — mpe ezali na mposa ya koyeba ndenge données esalemaki te.

**Ntalo:** mbongwana ya schema ya D1 etali ba fichiers mibale (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Assurance ya ntalo moke.

### Makambo oyo esengeli kosalema oyo etiami na bizaleli

- Ezali na boyokani na leta ya Amerika te; bilembo ya leta te.
- Makambo oyo elongolami na source ebombami, ezongisami sima te.
- Video ezali ya DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na site mobimba — ekoki kolukama na recherche, epekisami na AI-scrape.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
