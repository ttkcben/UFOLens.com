# GitHub — Færsla 3 af 3 · Athugasemdir um arkitektúr (ADR-stíls umræða)

**Nota sem:** Umræður undir „Sýna og segja frá“ / „Arkitektúr“, eða upphaf að ADR í `docs/`.
**Lykilorð:** arkitektúr, ADR, áfram-aðeins stöðuvél, staðbundið LLM, Ollama, OCR, brúnnet (edge computing), CSP, öryggisfyrirsagnir, gagnalögn, kostnaðarstjórnun, SQLite manifest, D1, R2, KV
**Tenglar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Af hverju ufolens.com er byggt eins og það er

Athugasemdir um þrjár ákvarðanir sem mótuðu [ufolens.com](https://www.ufolens.com) (leitanlega, fjöltyngda endurbyggingu á [PURSUE UAP skjalasafninu](https://www.war.gov/ufo)). Athugasemdir / mótrök vel þegin.

### 1. Pípulínan er áfram-aðeins stöðuvél — og það viljandi

Stöður: `discovered → downloaded → ocr_done → translated → published`. Skjal færist aðeins áfram, og aðeins þegar það er vinna sem þarf að vinna. Útgefið efni er aldrei endurunnið nema delta-skynjari sjái að frumskjalið hafi í raun breyst.

**Af hverju:** OCR + þýðing eru dýru aðgerðirnar, og skjalasafnið stækkar með tímanum. Pípulína sem „keyrir allt aftur til öryggis“ hefur ótakmarkaðan kostnað. Með því að gera afturvirkar færslur ómögulegar er ómögulegt að fá stjórnlausan reikning. Kostnaðarþakið er eiginleiki stöðuritsins, ekki árvekni stjórnanda.

**Kostnaður:** skemabreytingar og viljandi endurvinnsla eru af ásettu ráði óþjál. Ásættanleg málamiðlun.

### 2. OCR og þýðing keyra á staðbundnu LLM, ekki á skýja-API

OCR: opinn hugbúnaður, Tesseract CLI til vara. Þýðing + NER: Gemma í gegnum Ollama, á Apple Silicon fartölvu.

**Af hverju:** enginn jaðarkostnaður á skjal; endurgeranlegt (fast módel + leiðbeiningar); og sækingarskrefið verður hvort eð er að keyra frá heimilis-IP (frumskjalið er á bak við Akamai Bot Manager — `curl` fær 403), þannig að fartölva er í lykkjunni samt sem áður.

**Kostnaður:** þýðingargæðin eru undir fremstu módelum. Fyrir heimildasafn þar sem enska frumtextinn er alltaf aðeins einum smelli í burtu, þá er það í lagi. Við fullyrðum ekki að þýðingarnar séu marktækar.

### 3. Helmingarnir tveir deila nákvæmlega einu viðmóti: útgefnum pakka

Pípulínan skrifar aldrei beint í framleiðslugagnagrunninn. Hún gefur frá sér `{ SQL, eignamanifest, skyndiminni-hreinsunarlisti }`. „Að gefa út“ = að beita þeim pakka framvirkt (ýta SQL í brún-SQL-gagnagrunninn, samstilla eignir við hlutageymslu, hreinsa nefnda skyndiminni-lykla).

**Af hverju:** staðbundni hlutinn og brúnarhlutinn geta þróast óháð hvor öðrum; pakkinn er yfirfaranlegur; og „að dreifa gögnum“ er af sömu gerð í hvert skipti. Worker er lítið TypeScript/Hono forrit — strangt CSP (ekkert `unsafe-inline`; innbyggt JSON-LD er sha256-pinnað), `Accept-Language` + land→tungumál samningaviðræður, 30 daga KV skyndiminni fyrir síður, daglegur viðhalds-cron — og það þarf aldrei að vita hvernig gögnin urðu til.

**Kostnaður:** D1 skemabreyting snertir tvær skrár (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Lítill peningur fyrir góða tryggingu.

### Ófrávíkjanleg atriði innbyggð í hegðun

- Ekki tengt bandarískum stjórnvöldum; engin opinber merki.
- Yfirstrikanir í frumskjölum eru varðveittar, aldrei afmáðar.
- Myndbönd eignuð DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` á öllum vefnum — verðtrygganlegt af leitarvélum, afþakkað notkun í gervigreindarskrafi.

Í loftinu: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
