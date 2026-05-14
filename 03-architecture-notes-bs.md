# GitHub — Objava 3 od 3 · Bilješke o arhitekturi (diskusija u ADR stilu)

**Koristiti kao:** diskusiju pod "Pokaži i reci" / "Arhitektura", ili kao početnu tačku za ADR u docs/.
**Ključne riječi:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hiperveze:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Zašto je ufolens.com izgrađen na ovaj način

Bilješke o tri odluke koje su oblikovale [ufolens.com](https://www.ufolens.com) (pretraživu, višejezičnu preradu [PURSUE UAP arhiva](https://www.war.gov/ufo)). Komentari / kritike su dobrodošli.

### 1. Pipeline je mašina stanja "samo naprijed" — s namjerom

Stanja: `discovered → downloaded → ocr_done → translated → published`. Dokument se kreće samo naprijed, i to samo kada ima posla. Objavljeni sadržaj se nikada ponovo ne obrađuje osim ako detektor delte ne vidi da se izvor stvarno promijenio.

**Zašto:** OCR + prevođenje su skupe operacije, a arhiv s vremenom raste. Pipeline koji "ponovo pokreće sve da bi bio siguran" ima neograničene troškove. Onemogućavanje povratnih prijelaza čini nemogućim nekontrolirano povećanje računa. Gornja granica troškova je svojstvo grafa stanja, a ne budnosti operatera.

**Cijena:** migracije sheme i namjerno ponovno procesiranje su svjesno otežani. Prihvatljiv kompromis.

### 2. OCR i prevođenje se izvršavaju na lokalnom LLM-u, a ne na cloud API-ju

OCR: open-source engine, Tesseract CLI rezervna opcija. Prevođenje + NER: Gemma preko Ollama, na Apple Silicon laptopu.

**Zašto:** nula marginalnog troška po dokumentu; ponovljivost (fiksni model + promptovi); i korak dohvaćanja već mora biti izvršen s rezidencijalne IP adrese (izvor je iza Akamai Bot Managera — `curl` dobija 403), tako da je laptop svakako uključen u proces.

**Cijena:** kvaliteta prijevoda je ispod nivoa najnaprednijih modela. Za referentni korpus gdje je originalni engleski uvijek dostupan jednim klikom, to je u redu. Ne tvrdimo da su prijevodi autoritativni.

### 3. Dvije polovine dijele tačno jedno sučelje: objavljeni paket

Pipeline nikada ne piše direktno u produkcijsku bazu podataka. On emitira `{ SQL, manifest asseta, lista za čišćenje keša }`. "Objavljivanje" = primijeni taj paket (push SQL u edge SQL DB, sinkroniziraj assete s objektim skladištem, očisti imenovane ključeve keša).

**Zašto:** lokalna strana i edge strana mogu se razvijati neovisno; paket se može pregledati; i "postavljanje podataka" svaki put ima isti oblik. Worker je mala TypeScript/Hono aplikacija — strogi CSP (nema `unsafe-inline`; inline JSON-LD je sha256-prikačen), `Accept-Language` + pregovaranje zemlja→jezik, 30-dnevni KV keš stranica, dnevni cron za održavanje — i nikada ne treba znati kako su podaci nastali.

**Cijena:** promjena D1 sheme dotiče dva fajla (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Jeftino osiguranje.

### Neupitna pravila ugrađena u ponašanje

- Nije povezan s vladom SAD-a; nema službenih oznaka.
- Redakcije izvora se čuvaju, nikada ne poništavaju.
- Video pripisan DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na cijeloj stranici — indeksabilno za pretraživače, isključeno iz AI struganja.

Uživo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
