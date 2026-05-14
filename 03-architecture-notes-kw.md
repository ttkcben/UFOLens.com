# GitHub — Post 3 a 3 · Notennow Pennserneth (Keskows-stil ADR)

**Devnydh avel:** Keskows yn-dann "Diskwedhes ha derivya" / "Pennserneth", po hasen ADR rag `docs/`.
**Geryow alhwedh:** pennserneth, ADR, jynn studh rag-onlys, LLM leel, Ollama, OCR, amontya an amal, CSP, pennskrifow diogeledh, piblinell data, ynjynorieth kost, manifest SQLite, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Prag y feu ufolens.com drehevys y'n fordh ma

Notennow a-dro dhe'n tri divudh a furvyas [ufolens.com](https://www.ufolens.com) (an dasdrehevyans hwithradow, lies-yethek a'n [arhive PURSUE UAP](https://www.war.gov/ufo)). Komenow / dasgortheb a-gontra yw wolkom.

### 1. An biblinell yw jynn studh rag-onlys — a-borpos

Studhyow: `diskudhys → iskargys → ocr_gwres → treylys → dyllys`. Ny wra dokument marnas gwaya yn-rag, ha hepken pan vo ober dhe'y wul. Dalgh dyllys ny vo bythkweth dasoberys marnas diskudhher delta dhe weles an vammen dhe janjya yn gwir.

**Prag:** OCR + treylyans yw an oberow kostek, ha'n arhive a dyv dres termyn. Piblinell a "das-rol poptra rag bos saw" a's teves kost anhedhek. Dhe wul treflanow a-dhelergh anvurvys a wra faktur re-dreskas anvurvys. An nans kost yw gnas an graf studh, nyns yw a war-dybri an oberor.

**Kost:** gwayansow skema ha dasoberi a-borpos yw ankevywi a-dreveth. Kehevelepth akseptadow.

### 2. OCR ha treylyans a rol war LLM leel, nyns yw war API kommol

OCR: jynn mammen-yger, Tesseract CLI a-dhelergh. Treylyans + NER: Gemma dre Ollama, war laptop Apple Silicon.

**Prag:** kost marjynel mann dre dhokument; dasaskoradow (model + skrifennow fast); ha'n kamm `fetch` a res rolya seulabrys dhyworth IP trigys (an vammen yw a-dhelergh Akamai Bot Manager — `curl` a-degemer 403), ytho yma laptop y'n dolen seulabrys.

**Kost:** an kwalita treylyans yw isella ages model an or. Rag korpus-devyn a le mayth yw an Sowsnek derowel unn klik hepken, henna yw da lowr. Ny arghyn ni an treylyansow dhe vos awtoritaek.

### 3. An diw hanter a rann unn ynterfas hepken: bondel dyllys

Ny wra an biblinell bythkweth skrifa dhe'n database askorrans yn eeun. Ev a eskor `{ SQL, manifest asedhow, rol-purha cache }`. "Dyllo" = gorra an bondel na yn-rag (herdhya SQL dhe'n DB SQL amal, kestermyna asedhow dhe stokkas obyek, purha an alhwedhow cache henwys).

**Prag:** an tu leel ha'n tu amal a yll esplegya yn anserghek; an bondel yw adweladow; ha "delivra data" yw an keth furv pub tro. An Worker yw app byghan TypeScript/Hono — CSP strick (nyns eus `unsafe-inline`; JSON-LD a-linen yw sha256-pynnys), `Accept-Language` + kesvreusyans bro→yeth, cache folennow KV 30-dydh, cron gorweyth-chi pempenydhyek — ha ny'th esowgh dhe wodhvos bythkweth fatel veu an data gwrys.

**Kost:** chanj dhe skema D1 a doch dew fyll (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Surheans good-koop.

### An-geskowsadow pebys yn omdhegyans

- Nyns yw kevrynnys gans governans an S.U.; nyns eus arwodhow sodhogel.
- Digelennow an vammen yw gwithys, bythkweth treylys.
- Video kevys dhe DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` dres oll an savle — endeksadow gans jynnow hwilas, dewisys-mes a gravans AI.

Yn fyw: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
