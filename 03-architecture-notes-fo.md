# GitHub — Støða 3 av 3 · Arkitekturviðmerkingar (ADR-stíl Tjaktráður)

**Nýt sum:** ein Tjaktráður undir "Vís og fortel" / "Arkitekturur", ella sum byrjan til `docs/` ADR.
**Leitorð:** arkitekturur, ADR, einans-fram eftir gongandi støðumaskina, lokalur LLM, Ollama, OCR, edge computing, CSP, trygdarheaderar, daturørskipan, kostnaðarverkfrøði, SQLite manifest, D1, R2, KV
**Hyperleinkjur:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Hví ufolens.com er bygt, sum tað er

Viðmerkingar um tær tríggjar avgerðirnar, sum hava myndað [ufolens.com](https://www.ufolens.com) (tann leitanliga, fleirmálsliga endurbyggingin av [PURSUE UAP-skjalasavninum](https://www.war.gov/ufo)). Viðmerkingar / mótspæl eru vælkomin.

### 1. Rørskipanin er ein einans-fram eftir gongandi støðumaskina — við vilja

Støður: `uppdagað → niðurtikið → ocr_liðugt → týtt → útgivið`. Eitt skjal flytur seg bara framá, og bara tá ið arbeiði er at gera. Útgivið tilfar verður ongantíð viðgjørt av nýggjum, uttan so at ein delta-detektor sær, at keldan veruliga er broytt.

**Hví:** OCR + týðing eru tær dýru operationirnar, og skjalasavnið veksur við tíðini. Ein rørskipan, sum "endurkoyrir alt fyri at vera vís", hevur óavmarkaðan kostnað. At gera afturvendandi yvirgongdir ómøguligar ger ein óstýriligan rokning ómøguligan. Kostnaðarloftið er ein eginleiki hjá støðugraffinum, ikki hjá áræði hjá operatørinum.

**Kostnaður:** skemamigratiónir og endurviðgerð við vilja eru við vilja óhøgligar. Ein góðtakilig avveging.

### 2. OCR og týðing koyra á einum lokalum LLM, ikki einum ský-API

OCR: open-source motor, Tesseract CLI varaleið. Týðing + NER: Gemma via Ollama, á eini Apple Silicon farteldu.

**Hví:** null markkostnaður pr. skjal; endurgevandi (fast model + prompts); og fetch-stigið skal longu koyra frá eini privatari IP-adressu (keldan er aftan fyri Akamai Bot Manager — `curl` fær ein 403), so ein fartelda er longu í ringrásini.

**Kostnaður:** týðingarkvaliteturin er undir einum framúrskarandi modeli. Fyri eitt tilvísingarsavn, har upprunaliga enska altíð er ein klikk burturi, er tað í lagi. Vit pástanda ikki, at týðingarnar eru autoritativar.

### 3. Tær báðar helvtirnar deila nágreiniliga eitt markamót: ein útgivnan bundil

Rørskipanin skrivar ongantíð beinleiðis í framleiðslugrunnin. Hon útgevur `{ SQL, asset manifest, cache-purge listi }`. "At útgeva" = at nýta hendan bundilin framá (skumpa SQL til edge SQL DB, synkronisera assets til objektgoymslu, reinsa nevndu cache-lyklar).

**Hví:** lokala síðan og edge-síðan kunnu mennast óheft; bundilin kann gjøgnumskoðast; og "deploy data" hevur sama skap hvørja ferð. Worker er ein lítil TypeScript/Hono-app — strongur CSP (einki `unsafe-inline`; inline JSON-LD er sha256-fest), `Accept-Language` + land→mál samráðing, 30-daga KV-síðucache, dagligt húsarhalds-cron — og hon tørvar ongantíð at vita, hvussu data varð gjørt.

**Kostnaður:** ein D1-skemabroyting nemur við tvær fílur (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Bílig trygging.

### Ófrávíkilig krøv, ið eru innbygd í atferðina

- Ikki knýtt at amerikansku stjórnini; eingi almenn merki.
- Keldu-strikingar verða varðveittar, ongantíð umventar.
- Video tilskrivað DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` yvir alla síðuna — kann leiti-indekserast, frávalt frá AI-skaving.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
