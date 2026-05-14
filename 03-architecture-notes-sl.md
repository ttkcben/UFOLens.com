# GitHub — Objave 3 od 3 · Opombe o arhitekturi (razprava v slogu ADR)

**Uporaba:** kot razprava pod "Pokaži in povej" / "Arhitektura" ali kot osnutek za ADR v `docs/`.
**Ključne besede:** arhitektura, ADR, stanje stroja samo naprej, lokalni LLM, Ollama, OCR, robno računanje, CSP, varnostne glave, podatkovni cevovod, stroškovni inženiring, SQLite manifest, D1, R2, KV
**Hiperpovezave:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Zakaj je ufolens.com zgrajen tako, kot je

Opombe o treh odločitvah, ki so oblikovale [ufolens.com](https://www.ufolens.com) (iskalno, večjezično predelavo [arhiva PURSUE UAP](https://www.war.gov/ufo)). Komentarji / ugovori so dobrodošli.

### 1. Cevovod je namerno stanje stroja samo naprej

Stanja: `odkrit → prenesen → ocr_končan → preveden → objavljen`. Dokument se premika samo naprej in samo takrat, ko je treba opraviti delo. Objavljena vsebina se nikoli ponovno ne obdela, razen če detektor razlik opazi, da se je vir dejansko spremenil.

**Zakaj:** OCR + prevajanje sta dragi operaciji in arhiv sčasoma raste. Cevovod, ki "ponovno zažene vse, da bi bil varen", ima neomejene stroške. Onemogočanje prehodov nazaj onemogoča pobegli račun. Zgornja meja stroškov je lastnost grafa stanj, ne pa budnosti operaterja.

**Strošek:** migracije shem in namerna ponovna obdelava so namerno nerodne. Sprejemljiv kompromis.

### 2. OCR in prevajanje se izvajata na lokalnem LLM, ne na oblačnem API-ju

OCR: odprtokodni pogon, Tesseract CLI kot rezerva. Prevajanje + NER: Gemma prek Ollama, na prenosniku Apple Silicon.

**Zakaj:** ničelni mejni strošek na dokument; ponovljivost (fiksen model + pozivi); in korak pridobivanja mora že tako ali tako potekati z domačega IP-ja (vir je za Akamai Bot Manager — `curl` dobi 403), zato je prenosnik že vključen v zanko.

**Strošek:** kakovost prevoda je slabša od najsodobnejših modelov. Za referenčni korpus, kjer je izvirna angleščina vedno na voljo z enim klikom, je to v redu. Ne trdimo, da so prevodi avtoritativni.

### 3. Dve polovici si delita natanko en vmesnik: objavljen sveženj

Cevovod nikoli ne piše neposredno v produkcijsko bazo podatkov. Odda `{ SQL, manifest sredstev, seznam za praznjenje predpomnilnika }`. "Objavljanje" = uporabi ta sveženj naprej (potisni SQL v robno SQL bazo, sinhroniziraj sredstva v objektno shrambo, izprazni imenovane ključe predpomnilnika).

**Zakaj:** lokalna stran in robna stran se lahko razvijata neodvisno; sveženj je pregleden; in "uvajanje podatkov" ima vsakič enako obliko. Worker je majhna aplikacija TypeScript/Hono — strog CSP (brez `unsafe-inline`; vgrajeni JSON-LD je pripet s sha256), pogajanje `Accept-Language` + država→jezik, 30-dnevni KV predpomnilnik strani, dnevni vzdrževalni cron — in nikoli ne potrebuje vedeti, kako so bili podatki ustvarjeni.

**Strošek:** sprememba sheme D1 se dotika dveh datotek (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Poceni zavarovanje.

### Nespremenljiva pravila, vgrajena v obnašanje

- Ni povezan z vlado ZDA; brez uradnih oznak.
- Izvirne redakcije so ohranjene, nikoli razveljavljene.
- Video pripisan DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` po celotnem spletnem mestu — indeksirano s strani iskalnikov, izvzeto iz strganja AI.

V živo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

