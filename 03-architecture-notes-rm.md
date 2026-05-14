# GitHub — Post 3 da 3 · Notas d'architectura (Discussiun en stil d'ADR)

**Utilisar sco:** ina discussiun sut "Mussar e raquintar" / "Architectura", u sco emprim sboz d'ADR en `docs/`.
**Pleds-clav:** architectura, ADR, maschina da stadi che va be enavant, LLM local, Ollama, OCR, edge computing, CSP, headers da segirezza, pipeline da datas, enginneria da custs, manifest da SQLite, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Pertge che ufolens.com è construì uschia

Notas davart las trais decisiuns ch'han furmà [ufolens.com](https://www.ufolens.com) (la reconstrucziun tschertgabla e plurilingua da l'[archiv PURSUE UAP](https://www.war.gov/ufo)). Comments / critica èn bainvegnids.

### 1. La pipeline è ina maschina da stadi che va be enavant — sapientivamain

Stadis: `scuvert → telechargià → ocr_fatg → translatà → publitgà`. In document sa mova be enavant, e be sch'i dat da far insatge. Cuntegn publitgà na vegn mai pli elavurà, nun che in detectur da delta vesa che la funtauna è effectivamain sa midada.

**Pertge:** OCR + translaziun èn las operaziuns charas, e l'archiv crescha cun il temp. Ina pipeline che "fa tut da nov per esser segir" ha custs illimitads. Impedir transiziuns enavos renda in quint exorbitant nunpussaivel. Il plafun dals custs è ina caracteristica dal graf da stadi, betg da la vigilanza da l'operatur.

**Cust:** migraziuns da schema e re-elavuraziuns intenziunadas èn deliberadamain malcumadaivlas. In cumpromiss acceptabel.

### 2. OCR e translaziun funcziuneschan sin in LLM local, betg in'API da cloud

OCR: engine open-source, fallback sin Tesseract CLI. Translaziun + NER: Gemma via Ollama, sin in laptop Apple Silicon.

**Pertge:** nagins custs marginals per document; reproducibel (model fix + prompts); ed il pass da fetch sto gia vegnir exequì d'in IP residenzial (la funtauna è davos Akamai Bot Manager — `curl` survegn in 403), pia è in laptop in ogni cas en il loop.

**Cust:** la qualitad da la translaziun è sut in model da punta. Per in corpus da referenza nua che l'original englais è adina be in clic davent, è quai en urden. Nus na pretendain betg che las translaziuns sajan autoritativas.

### 3. Las duas mesadads partan exact ina interfatscha: in pachet publitgà

La pipeline na scriva mai directamain en la banca da datas da producziun. Ella emetta `{ SQL, manifest d'assets, glista da purgar la cache }`. "Publitgar" = applicar quest pachet enavant (puschar SQL en la DB SQL da l'edge, sincronisar assets en la memoria d'objects, purgar las clavs da cache numnadas).

**Pertge:** la vart locala e la vart da l'edge pon evoluar independentamain; il pachet è recensibel; e "deployar datas" ha mintgamai la medema furma. Il Worker è ina pitschna applicaziun da TypeScript/Hono — CSP sever (nagin `unsafe-inline`; JSON-LD inline è cun sha256-fixà), negoziaziun `Accept-Language` + pajais→lingua, cache da pagina da 30 dis en KV, cron da mantegniment da mintga di — ed el na sto mai savair co las datas èn vegnidas creadas.

**Cust:** ina midada al schema da D1 tocca dus files (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). In'assicuranza bunmartgada.

### Cundiziuns inviolablas integradas en il cumportament

- Betg affilià cun la regenza dals Stadis Unids; nagins insigns uffizials.
- Redacziuns da funtauna vegnan mantegnidas, mai revocadas.
- Video attribuì a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` per l'entira pagina — indexabel per maschinas da tschertga, opt-out per scraping d'AI.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
