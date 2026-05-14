# GitHub — Nuntius III ex III · Notae de architectura (Disputatio stili ADR)

**Uti ut:** disputatio sub "Ostende et narra" / "Architectura", aut semen pro `docs/` ADR.
**Claves verborum:** architectura, ADR, machina statuum quae solum progreditur, LLM localis, Ollama, OCR, computatio in margine, CSP, capitis securitatis, catena operum datorum, ingeniaria sumptuum, manifestum SQLite, D1, R2, KV
**Hypertextus:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cur ufolens.com aedificatum est sicut est

Notae de tribus decisionibus quae [ufolens.com](https://www.ufolens.com) formaverunt (re-aedificatio pervestigabilis et multilinguis [archivi PURSUE UAP](https://www.war.gov/ufo)). Commentarii / contradictiones gratae sunt.

### 1. Catena operum est machina statuum quae solum progreditur — ex proposito

Status: `inventum → receptum → ocr_perfectum → translatum → publicatum`. Documentum solum progreditur, et solum cum opus est faciendum. Contentum publicatum numquam denuo tractatur, nisi detector differentiarum fontem re vera mutatum viderit.

**Cur:** OCR + translatio sunt operationes sumptuosae, et archivum cum tempore crescit. Catena operum quae "omnia denuo currit ut tuta sit" sumptum infinitum habet. Transitiones retrogradas impossibiles facere efficit ut rogatio pecuniaria ex controlle exiens impossibilis sit. Tectum sumptuum est proprietas graphi statuum, non vigilantiae operatoris.

**Sumptus:** migrationes schematis et re-processus ex proposito deliberate incommodi sunt. Commutatio acceptabilis.

### 2. OCR et translatio in LLM locali currunt, non in API nubis

OCR: machina fontis aperti, Tesseract CLI subsidiaria. Translatio + NER: Gemma per Ollama, in laptop Apple Silicon.

**Cur:** nullus sumptus marginalis per documentum; reproducibilis (exemplar fixum + prompta); et gradus captionis iam currere debet ab IP residentiali (fons post Akamai Bot Manager est — `curl` accipit 403), ergo laptop iam in circuitu est.

**Sumptus:** qualitas translationis inferior est quam exemplar in fronte. Pro corpore referentiae ubi textus originalis Anglicus semper uno clicculo abest, hoc tolerabile est. Non affirmamus translationes esse auctoritativas.

### 3. Duae partes communicant unam tantum interfaciem: fasciculum publicatum

Catena operum numquam scribit in basim datorum productionis directe. Emittit `{ SQL, manifestum bonorum, indicem purgationis cache }`. "Publicare" = illum fasciculum progrediendo applicare (SQL ad DB SQL in margine impellere, bona ad repositorium obiectorum synchronizare, claves cache nominatas purgare).

**Cur:** latus locale et latus in margine independenter evolvi possunt; fasciculus recenseri potest; et "data explicare" eandem formam omni tempore habet. Worker est parva applicatio TypeScript/Hono — strictum CSP (nullum `unsafe-inline`; JSON-LD in linea per sha256-fixum), `Accept-Language` + negotiatio natio→lingua, cache paginarum in KV per 30 dies, cron quotidianum ad sustentationem — et numquam scire debet quomodo data facta sint.

**Sumptus:** mutatio schematis D1 duos fasciculos tangit (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Assecuratio vilis.

### Incommutabilia in mores infixa

- Non affiliatum gubernationi Civitatum Foederatarum; nulla insignia officialia.
- Redactiones fontis servantur, numquam invertuntur.
- Video attributum DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` per totum situm — indicabilis a machinis quaestionis, AI-exscriptione-optatus-ex.

In vivo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

