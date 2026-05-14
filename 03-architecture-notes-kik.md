# GitHub — Kibandi 3 harĩ 3 · Ndũmĩrĩri cia Mũhianĩre (ADR-style Discussion)

**Hũthĩra ta:** Discussion rungu rwa "Kuonania na Kuira" / "Mũhianĩre", kana `docs/` ADR kĩambĩrĩria.
**Ciugo cia bata:** mũhianĩre, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Gĩtũmi kĩrĩa gĩtũmĩte ufolens.com ĩthondeketwo ũrĩa ĩthondeketwo

Ndũmĩrĩri igũrũ rĩa matua matatũ marĩa mathondekire [ufolens.com](https://www.ufolens.com) (gũthondeka kwerũ kĩa gũcaria maũndũ, kwa ndimi nyingĩ kĩa mũthithũ wa [PURSUE UAP](https://www.war.gov/ufo)). Ciugo cia kwongerera / kũregana nĩ ciamwarĩrwo.

### 1. Pipeline nĩ mũtaratara wa forward-only wa state — na kĩgendi

Tũtara: `discovered → downloaded → ocr_done → translated → published`. Ndumenti ĩthiaga mbere tu, na o rĩrĩa kũrĩ wĩra wa kũrutwo. Indo iria ciathirĩirio iticokerithagio wĩra tiga o korwo kĩonjoria kĩa delta kĩona atĩ kĩhumo nĩ kĩgarũrũkĩte.

**Gĩtũmi:** OCR + ũtafsiri nĩcio ciĩko cia goro, na mũthithũ ũkũraga na ihinda. Pipeline ĩrĩa "ĩcokerithagia wĩra wothe nĩguo ĩkorwo na ũgitĩri" ĩrĩ na garama ĩtarĩ na mĩhaka. Gũtũma mogarũrũku ma thutha matangĩhoteka nĩ gũtũmaga rĩpoti ya garama nene ĩtagĩka. Mĩhaka ya garama nĩ kĩrũmbũiyo kĩa gĩthima kĩa state, ti kĩa kũmenyerera kwa mũrutithia wĩra.

**Garama:** mogarũrũku ma schema na wĩra wa gũcokerera wĩra na kĩgendi nĩ mĩritũ na kĩgendi. Nĩ ngurani ĩtĩkĩrĩkĩte.

### 2. OCR na ũtafsiri irutagĩrwo wĩra na LLM ya kũu, ti na cloud API

OCR: injini ya open-source, fallback ya Tesseract CLI. Ũtafsiri + NER: Gemma kũgerera Ollama, thĩinĩ wa laptop ya Apple Silicon.

**Gĩtũmi:** gũtirĩ garama ya kwongerera kwa kila ndumenti; nĩ ya gũcokerereka (mũhiano na prompt iria itagarũrũkaga); na gĩcunjĩ kĩa gũkuua gĩkĩrabatara kũrutwo wĩra kuuma IP ya mũciĩ (kĩhumo kĩrĩ thutha wa Akamai Bot Manager — `curl` yonaga 403), kwoguo laptop nĩ ĩrabatarania o ũguo.

**Garama:** ũhoti wa ũtafsiri ũrĩ thĩ wa mũhiano wa gĩkĩro kĩa igũrũ. Harĩ mũthithũ wa kũringithania harĩa Gĩthũngũ kĩa kĩambĩrĩria kĩrĩ o gatagatĩ kamwe tu, ũcio nĩ wega. Tũtiugaga atĩ ũtafsiri ũcio nĩ wa kwĩhoko.

### 3. Ciondo ici igĩrĩ igayanaga na njĩra o ĩmwe tu: mũkũnga-watho wathirĩirio

Pipeline ndĩrĩ hĩndĩ yandĩkaga database ya production rũsũmũ. Ĩmathiragia `{ SQL, asset manifest, cache-purge list }`. "Gũmathiriria" = gũthondeka mũkũnga-watho ũcio mbere (gũikũria SQL nginya edge SQL DB, gũthondeka indo nginya hũngĩro ya indo, kũheria cihembe cia cache iria ciugĩtwo).

**Gĩtũmi:** gĩcunjĩ kĩa kũu na gĩa edge no ihote gũkũra na njĩra ya mwanya; mũkũnga-watho nĩ wa kũthuthurika; na "data ya kũrũgamia" ĩrĩ na mũhianĩre ũmwe o hĩndĩ. Worker nĩ app nini ya TypeScript/Hono — CSP ya hali ya igũrũ (gũtirĩ `unsafe-inline`; inline JSON-LD nĩ sha256-pinned), `Accept-Language` + kwarĩrĩrio kwa bũrũri→rũrĩmĩ, cache ya peji ya KV ya thikũ 30, cron ya wĩra wa kĩndũ o mũthenya — na ndĩrabatara kũmenya ũrĩa data yathondekirwo.

**Garama:** mogarũrũku ma D1 schema mahutagia faili igĩrĩ (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Nĩ inshuwarasi ya bei ya thĩ.

### Maũndũ matarĩ ma kwarĩrĩrio mathondeketwo thĩinĩ wa wĩtwarari

- Ndũna uhusiano na thirikari ya U.S.; gũtirĩ rũũri rwa kĩthirikari.
- Macacĩ ma kĩambĩrĩria nĩmatigagwo, matirĩ hĩndĩ magarũrũkagio.
- Video nĩ cia DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` saitĩ-inĩ yothe — ĩhotithagio gũciona nĩ injini cia gũcaria, nĩ yetangĩte kuuma kũguĩo nĩ AI.

Rĩu rĩrĩ hewa-inĩ: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
