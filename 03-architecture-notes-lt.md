# GitHub – 3 iš 3 įrašų · Architektūros pastabos (ADR stiliaus diskusija)

**Naudoti kaip:** diskusiją skiltyje „Parodyk ir papasakok“ / „Architektūra“ arba `docs/` ADR pagrindą.
**Raktažodžiai:** architektūra, ADR, tik pirmyn būsenų mašina, vietinis LLM, Ollama, OCR, edge computing, CSP, saugumo antraštės, duomenų dutotiekis, sąnaudų inžinerija, SQLite manifestas, D1, R2, KV
**Hipersaitai:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kodėl ufolens.com sukurta būtent taip

Pastabos apie tris sprendimus, suformavusius [ufolens.com](https://www.ufolens.com) (paieškos galimybę turinčią, daugiakalbę [PURSUE UAP archyvo](https://www.war.gov/ufo) rekonstrukciją). Komentarai / kritika laukiami.

### 1. Dutotiekis yra sąmoningai sukurta „tik į priekį“ būsenų mašina

Būsenos: `discovered → downloaded → ocr_done → translated → published`. Dokumentas juda tik pirmyn ir tik tada, kai yra darbo, kurį reikia atlikti. Paskelbtas turinys niekada neperdirbamas, nebent delta detektorius pamato, kad pirminis šaltinis iš tikrųjų pasikeitė.

**Kodėl:** OCR + vertimas yra brangios operacijos, o archyvas laikui bėgant auga. Dutotiekis, kuris „viską paleidžia iš naujo dėl saugumo“, turi neapibrėžtas sąnaudas. Padarius atgalinius perėjimus neįmanomais, neįmanoma tampa ir nekontroliuojama sąskaita. Sąnaudų lubos yra būsenų grafiko savybė, o не operatoriaus budrumo.

**Kaina:** schemos migracijos ir tikslingas perdirbimas yra sąmoningai nepatogūs. Priimtinas kompromisas.

### 2. OCR ir vertimas veikia vietiniame LLM, o ne debesijos API

OCR: atvirojo kodo variklis, Tesseract CLI atsarginis variantas. Vertimas + NER: Gemma per Ollama, Apple Silicon nešiojamajame kompiuteryje.

**Kodėl:** nulinės papildomos sąnaudos vienam dokumentui; atkuriamumas (fiksuotas modelis + užklausos); ir paėmimo žingsnis jau turi būti vykdomas iš gyvenamosios vietos IP (šaltinis yra už Akamai Bot Manager – `curl` gauna 403), todėl nešiojamasis kompiuteris vis tiek yra procese.

**Kaina:** vertimo kokybė yra prastesnė nei pažangiausio modelio. Referenciniam korpusui, kur originali anglų kalba visada pasiekiama vienu paspaudimu, tai tinka. Mes neteigiame, kad vertimai yra autoritetingi.

### 3. Abi pusės dalijasi lygiai viena sąsaja: paskelbtu paketu

Dutotiekis niekada nerašo tiesiai į gamybinę duomenų bazę. Jis išveda `{ SQL, išteklių manifestas, talpyklos valymo sąrašas }`. „Skelbimas“ = pritaikyti tą paketą pirmyn (įkelti SQL į edge SQL DB, sinchronizuoti išteklius į objektų saugyklą, išvalyti nurodytus talpyklos raktus).

**Kodėl:** vietinė ir edge pusės gali vystytis nepriklausomai; paketas yra peržiūrimas; ir „įdiegti duomenis“ kaskart yra tos pačios formos. Worker yra maža TypeScript/Hono programa – griežtas CSP (jokio `unsafe-inline`; įterptas JSON-LD yra sha256-prisegtas), `Accept-Language` + šalies→kalbos derinimas, 30 dienų KV puslapių talpykla, kasdienis tvarkymo cron darbas – ir jai niekada nereikia žinoti, kaip duomenys buvo sukurti.

**Kaina:** D1 schemos pakeitimas liečia du failus (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Pigus draudimas.

### Neginčytini principai, integruoti į elgseną

- Nėra susijęs su JAV vyriausybe; jokios oficialios simbolikos.
- Pirminio šaltinio redagavimai išsaugomi, niekada neatšaukiami.
- Vaizdo įrašai priskiriami DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` visoje svetainėje – indeksuojama paieškos sistemų, bet atsisakyta AI duomenų rinkimui.

Veikia: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

