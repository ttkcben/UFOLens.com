# GitHub – Príspevok 3 z 3 · Architektonické poznámky (Diskusia v štýle ADR)

**Použitie ako:** diskusia v sekcii "Show and tell" / "Architecture" alebo základ pre ADR v `docs/`.
**Kľúčové slová:** architektúra, ADR, jednosmerný stavový automat, lokálne LLM, Ollama, OCR, edge computing, CSP, bezpečnostné hlavičky, dátový pipeline, nákladové inžinierstvo, SQLite manifest, D1, R2, KV
**Hypertextové odkazy:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Prečo je ufolens.com postavený tak, ako je

Poznámky k trom rozhodnutiam, ktoré formovali [ufolens.com](https://www.ufolens.com) (prehľadávateľnú, viacjazyčnú rekonštrukciu archívu [PURSUE UAP](https://www.war.gov/ufo)). Komentáre / kritika sú vítané.

### 1. Pipeline je zámerne jednosmerný stavový automat

Stavy: `discovered → downloaded → ocr_done → translated → published`. Dokument sa posúva iba dopredu a iba vtedy, keď je čo robiť. Publikovaný obsah sa nikdy nespracúva znova, pokiaľ detektor delta nezistí, že sa zdroj skutočne zmenil.

**Prečo:** OCR + preklad sú nákladné operácie a archív časom rastie. Pipeline, ktorý "pre istotu spúšťa všetko znova", má neobmedzené náklady. Znemožnenie spätných prechodov znemožňuje nekontrolovateľný účet. Strop nákladov je vlastnosťou stavového grafu, nie ostražitosti operátora.

**Náklady:** migrácie schémy a zámerné opätovné spracovanie sú úmyselne nepohodlné. Prijateľný kompromis.

### 2. OCR a preklad bežia na lokálnom LLM, nie na cloudovom API

OCR: open-source engine, Tesseract CLI ako záloha. Preklad + NER: Gemma cez Ollama, na laptope s Apple Silicon.

**Prečo:** nulové marginálne náklady na dokument; reprodukovateľné (fixný model + prompt); a krok sťahovania už musí bežať z rezidenčnej IP adresy (zdroj je za Akamai Bot Manager — `curl` dostane 403), takže laptop je tak či tak v hre.

**Náklady:** kvalita prekladu je nižšia ako pri špičkovom modeli. Pre referenčný korpus, kde je pôvodná angličtina vždy na dosah jedným kliknutím, je to v poriadku. Netvrdíme, že preklady sú autoritatívne.

### 3. Obe polovice zdieľajú presne jedno rozhranie: publikovaný balík

Pipeline nikdy nezapisuje priamo do produkčnej databázy. Vytvára `{ SQL, manifest assetov, zoznam na vymazanie cache }`. "Publikovanie" = aplikovať tento balík dopredu (poslať SQL do edge SQL DB, synchronizovať assety do objektového úložiska, vymazať pomenované kľúče z cache).

**Prečo:** lokálna a edge strana sa môžu vyvíjať nezávisle; balík je kontrolovateľný; a "nasadenie dát" má vždy rovnaký formát. Worker je malá aplikácia v TypeScript/Hono — prísne CSP (žiadne `unsafe-inline`; inline JSON-LD je pripnutý pomocou sha256), dohadovanie jazyka cez `Accept-Language` + krajina→jazyk, 30-dňová KV cache pre stránky, denný cron na údržbu — a nikdy nepotrebuje vedieť, ako dáta vznikli.

**Náklady:** zmena D1 schémy sa dotýka dvoch súborov (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Lacná poistka.

### Neoddiskutovateľné princípy zabudované v správaní

- Nie je spojený s vládou USA; žiadne oficiálne insígnie.
- Začiernené texty v zdrojoch sú zachované, nikdy nie odstraňované.
- Video je pripísané DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na celej stránke — indexovateľné pre vyhľadávače, vylúčené zo scrapovania AI.

Naživo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

