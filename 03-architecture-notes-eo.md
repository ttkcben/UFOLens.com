# GitHub — Afiŝo 3 el 3 · Arkitekturaj notoj (Diskuto en ADR-stilo)

**Uzu kiel:** diskuto sub "Montri kaj rakonti" / "Arkitekturo", aŭ komencaĵo por ADR en `docs/`.
**Ŝlosilvortoj:** arkitekturo, ADR, nur-antaŭen statmaŝino, loka LLM, Ollama, OCR, randa komputado, CSP, sekurecaj kaplinioj, datuma dukto, kosta inĝenierado, SQLite-manifesto, D1, R2, KV
**Hiperligoj:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kial ufolens.com estas konstruita tiel, kiel ĝi estas

Notoj pri la tri decidoj, kiuj formis [ufolens.com](https://www.ufolens.com) (la serĉebla, plurlingva rekonstruo de la arkivo [PURSUE pri UAP](https://www.war.gov/ufo)). Komentoj / kontraŭargumentoj estas bonvenaj.

### 1. La dukto estas nur-antaŭen statmaŝino — intence

Statoj: `malkovrita → elŝutita → ocr_farita → tradukita → publikigita`. Dokumento nur moviĝas antaŭen, kaj nur kiam estas laboro farenda. Publikigita enhavo neniam estas reprocesita krom se diferenc-detektilo vidas, ke la fonto efektive ŝanĝiĝis.

**Kial:** OCR + tradukado estas la multekostaj operacioj, kaj la arkivo kreskas laŭlonge de la tempo. Dukto, kiu "reruliĝas ĉion por esti sekura" havas senliman koston. Malebligi retroirajn transirojn malebligas senbridan fakturon. La kosta plafono estas eco de la statgrafeo, ne de la atentemo de la operacianto.

**Kosto:** skemaj migradoj kaj intenca reprocesado estas intence mallertaj. Akceptebla kompromiso.

### 2. OCR kaj tradukado ruliĝas sur loka LLM, ne nuba API

OCR: malfermfonta motoro, Tesseract CLI rezervo. Tradukado + NER: Gemma per Ollama, sur Apple Silicon-portebla komputilo.

**Kial:** nul marĝena kosto por ĉiu dokumento; reproduktebla (fiksa modelo + instigoj); kaj la kapta paŝo jam devas ruliĝi de hejma IP (la fonto estas malantaŭ Akamai Bot Manager — `curl` ricevas 403), do portebla komputilo estas ĉiuokaze en la ciklo.

**Kosto:** la tradukkvalito estas sub la nivelo de avangarda modelo. Por referenca korpuso kie la originala angla ĉiam estas je unu klako for, tio estas en ordo. Ni ne pretendas, ke la tradukoj estas aŭtoritataj.

### 3. La du duonoj kunhavas precize unu interfacon: publikigitan pakaĵon

La dukto neniam skribas rekte al la produkta datumbazo. Ĝi eldonas `{ SQL, aktivaĵa manifesto, listo por kaŝmemor-purigado }`. "Publikigado" = apliki tiun pakaĵon antaŭen (puŝi SQL al la randa SQL DB, sinkronigi aktivaĵojn al objekta stokado, purigi la nomitajn kaŝmemorajn ŝlosilojn).

**Kial:** la loka flanko kaj la randa flanko povas evolui sendepende; la pakaĵo estas reviziebla; kaj "deploji datumojn" havas la saman formon ĉiufoje. La `Worker` estas malgranda TypeScript/Hono-aplikaĵo — strikta CSP (neniu `unsafe-inline`; enlinia JSON-LD estas fiksita per `sha256`), `Accept-Language` + lando→lingva negocado, 30-taga KV-paĝa kaŝmemoro, ĉiutaga puriga kron-tasko — kaj ĝi neniam bezonas scii kiel la datumoj estis kreitaj.

**Kosto:** `D1` skema ŝanĝo tuŝas du dosierojn (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Malmultekosta asekuro.

### Nediskuteblaj aferoj enkonstruitaj en la konduto

- Ne ligita al la usona registaro; neniuj oficialaj insignoj.
- Fontaj redaktoj estas konservitaj, neniam malfaritaj.
- Video atribuita al DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` tra la tuta retejo — serĉ-indeksebla, AI-skrap-malaliĝita.

Retejo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

