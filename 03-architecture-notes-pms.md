# GitHub — Publicassion 3 ëd 3 · Nòte d'architetura (Discussion stil ADR)

**Dovré com:** na Discussion sota "Smon-me e dime" / "Architetura", o 'me sëmina ADR për `docs/`.
**Paròle ciav:** architetura, ADR, màchina a stat mach anans, LLM local, Ollama, OCR, edge computing, CSP, antestassion ëd sigurëssa, pipeline ëd dàit, ingegnerìa dij cost, manifest SQLite, D1, R2, KV
**Anliure:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Përchè ufolens.com a l'é costruì parèj

Nòte an sle tre decision ch'a l'han formà [ufolens.com](https://www.ufolens.com) (la ripublicassion sërcàbil e multilingua dl'archiv [PURSUE UAP](https://www.war.gov/ufo)). Coment / obession a son bin accetà.

### 1. La pipeline a l'é na màchina a stat mach anans — a pòsta

Stat: `dëscoatà → dëscarià → ocr_fait → traducì → publicà`. Un document a va mach anans, e mach quand ch'a-i é ëd travaj da fé. Ël contnù publicà a l'é mai pì rielaborà, gavà che un detetor ëd delta a vëggia che la sorgiss a l'é cambià da da bon.

**Përchè:** OCR e tradussion a son j'operassion pi care, e l'archiv a chërs con ël temp. Na pipeline ch'a "fa torné tut për sigurëssa" a l'ha un cost ilimità. Rende impossìbij le transission a l'andarè a rend impossìbil na spèisa fòra contròl. Ël lìmit massimal ëd cost a l'é na propietà dël graf ëd jë stat, nen dla vigilansa dl'operador.

**Cost:** le migrassion dlë schema e l'rielaborassion antensional a son fàite dësvantagiose a pòsta. Un compromess acetàbil.

### 2. OCR e tradussion a giro su un LLM local, nen su n'API cloud

OCR: motor open-source, Tesseract CLI 'me riserva. Tradussion + NER: Gemma via Ollama, su un laptop Apple Silicon.

**Përchè:** cost marginal zero për document; riproducìbil (model e prompt fiss); e la fasa ëd letura a dev già giré da n'IP residensial (la sorgiss a l'é daré Akamai Bot Manager — `curl` a pija un 403), donca un laptop a l'é già implicà.

**Cost:** la qualità dla tradussion a l'é sota un model ëd ponta. Për n'archiv ëd referensa andoa che l'original anglèis a l'é sèmper a un clic ëd distansa, a va bin. I diso pa che le tradussion a son autorévole.

### 3. Le doe mità a condivido mach n'interfacia: un pachet publicà

La pipeline a scriv mai diretament ant la base ëd dàit ëd produssion. A produv `{ SQL, manifest dj'asset, lista ëd purge dla cache }`. "Publiché" = apliché col pachet anans (possé l'SQL a la base ëd dàit SQL edge, sincronisé j'asset an sla memòria d'oget, dëscanselé le ciav ëd cache nominà).

**Përchè:** la part local e la part edge a peulo evolvse an manera indipendenta; ël pachet a peul esse revisionà; e "deployé ij dàit" a l'ha sèmper la midema forma. Ël Worker a l'é na cita aplicassion TypeScript/Hono — CSP sever (gnun `unsafe-inline`; ël JSON-LD an linia a l'é fissà con sha256), negossiassion `Accept-Language` + pais→lenga, cache ëd pàgina KV ëd 30 dì, cron ëd polissìa giornalié — e a l'ha mai da manca ëd savej com che ij dàit a son stàit creà.

**Cost:** na modìfica al schema D1 a toca doi file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). N'assicurassion ch'a costa pòch.

### Prinsipi nen negossiàbij ancorporà ant ël comportament

- Nen afilià con ël goern djë Stat Unì; gnun-a insigna ofissial.
- Le redassion sorgiss a son conservà, mai anulà.
- Video atribuì a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` an tut ël sìt — indicisàbil da ij motor d'arserca, gavà da la racòlta dàit për l'IA.

An diresta: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

