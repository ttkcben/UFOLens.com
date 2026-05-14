# GitHub — Objava 3 od 3 · Arhitektonske bilješke (rasprava u ADR stilu)

**Koristiti kao:** raspravu pod "Show and tell" / "Arhitektura" ili kao početnu točku za ADR u `docs/`.
**Ključne riječi:** arhitektura, ADR, stroj stanja samo naprijed, lokalni LLM, Ollama, OCR, rubno računalstvo, CSP, sigurnosna zaglavlja, cjevovod podataka, inženjering troškova, SQLite manifest, D1, R2, KV
**Poveznice:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Zašto je ufolens.com izgrađen na ovaj način

Bilješke o tri odluke koje su oblikovale [ufolens.com](https://www.ufolens.com) (pretraživu, višejezičnu ponovnu izgradnju arhive [PURSUE UAP](https://www.war.gov/ufo)). Komentari / kritike su dobrodošli.

### 1. Cjevovod je namjerno stroj stanja koji ide samo naprijed

Stanja: `discovered → downloaded → ocr_done → translated → published`. Dokument se kreće samo naprijed i samo kada ima posla za obaviti. Objavljeni sadržaj se nikada ponovno ne obrađuje osim ako detektor delte ne vidi da se izvor stvarno promijenio.

**Zašto:** OCR + prevođenje su skupe operacije, a arhiva s vremenom raste. Cjevovod koji "sve ponovno pokreće da bi bio siguran" ima neograničene troškove. Onemogućavanje prijelaza unatrag onemogućuje nekontrolirani račun. Gornja granica troškova svojstvo je grafa stanja, a ne budnosti operatera.

**Cijena:** migracije shema i namjerna ponovna obrada su hotimice nezgrapni. Prihvatljiv kompromis.

### 2. OCR i prevođenje se izvršavaju na lokalnom LLM-u, a ne na API-ju u oblaku

OCR: open-source mehanizam, Tesseract CLI kao rezerva. Prevođenje + NER: Gemma putem Ollama, na Apple Silicon prijenosnom računalu.

**Zašto:** nulti marginalni trošak po dokumentu; ponovljivost (fiksni model + promptovi); a korak dohvaćanja već se mora izvršiti s kućnog IP-a (izvor je iza Akamai Bot Managera — `curl` dobiva 403), tako da je prijenosno računalo ionako u petlji.

**Cijena:** kvaliteta prijevoda je ispod razine najnaprednijih modela. Za referentni korpus gdje je originalni engleski uvijek na jedan klik udaljenosti, to je u redu. Ne tvrdimo da su prijevodi autoritativni.

### 3. Dvije polovice dijele točno jedno sučelje: objavljeni paket

Cjevovod nikada ne piše izravno u produkcijsku bazu podataka. On emitira `{ SQL, manifest resursa, popis za čišćenje predmemorije }`. "Objavljivanje" = primijeni taj paket prema naprijed (gurni SQL u rubnu SQL bazu podataka, sinkroniziraj resurse u pohranu objekata, očisti imenovane ključeve predmemorije).

**Zašto:** lokalna strana i rubna strana mogu se razvijati neovisno; paket se može pregledati; a "postavljanje podataka" svaki put ima isti oblik. Worker je mala TypeScript/Hono aplikacija — strog CSP (bez `unsafe-inline`; inline JSON-LD je sha256-prikvačen), `Accept-Language` + pregovaranje zemlja→jezik, 30-dnevna KV predmemorija stranica, dnevni cron za održavanje — i nikada ne treba znati kako su podaci nastali.

**Cijena:** promjena D1 sheme dotiče dvije datoteke (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Jeftino osiguranje.

### Neupitna pravila ugrađena u ponašanje

- Nije povezan s vladom SAD-a; bez službenih oznaka.
- Redakcije izvora su sačuvane, nikad se ne poništavaju.
- Video pripisan DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na cijeloj stranici — indeksira se za pretraživače, isključen iz AI struganja.

Uživo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

