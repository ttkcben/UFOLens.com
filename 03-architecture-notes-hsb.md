# GitHub — Přispawk 3 z 3 · Architekturne noticy (Diskusija w ADR-stilje)

**Wužij jako:** Diskusija pod "Pokazać a rozprawjeć" / "Architektura" abo jako wuchadźišćo za `docs/` ADR.
**Klučowe hesła:** architektura, ADR, mašina z stawom jenož doprědka, lokalny LLM, Ollama, OCR, edge computing, CSP, wěstotne hłowy, datowy pipeline, kóštowa inženjerija, SQLite-manifest, D1, R2, KV
**Wotkazy:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Čehodla je ufolens.com tak twarjeny, kaž je

Noticy wo třoch rozsudach, kotrež su [ufolens.com](https://www.ufolens.com) (přepytać dacace so, wjacerěčne znowa twarjenje [PURSUE UAP-archiwa](https://www.war.gov/ufo)) formowali. Komentary / přećiwna kritika wutrobnje witane.

### 1. Pipeline je mašina z stawom, kotraž dźěła jenož doprědka — z wotpohladom

Stawy: `namakany → sćehnjeny → ocr_sčinjeny → přełoženy → wozjewjeny`. Dokument so jenož doprědka pohibuje, a to jenož, hdyž je dźěło. Wozjewjeny wobsah so ženje znowa njepředźěłuje, chibazo delta-detektor widźi, zo je so žórło woprawdźe změniło.

**Čehodla:** OCR + přełožk stej drohej operaciji, a archiw rosće z časom. Pipeline, kotryž "wšo znowa přeběži, zo by wěsty był", ma njewobmjezowane kóšty. Znjemóžnjenje wróćo pohibow znjemóžnja njekontrolowany ličbu. Kóštowy strop je kajkosć stataweho grafa, nic čujnosće operatora.

**Kóšty:** šemowe migracije a znowapředźěłanje z wotpohladom su zmysłapołnje njewobratne. Akceptujomny kompromis.

### 2. OCR a přełožk běžitej na lokalnym LLM, nic na mróčelnym API

OCR: open-source engine, Tesseract CLI fallback. Přełožk + NER: Gemma přez Ollama, na laptopje Apple Silicon.

**Čehodla:** nulowe marginalne kóšty na dokument; reproducěrujomny (fiksnym model + pokiwy); a krok sćehnjenja dyrbi hižo z bydlenskeho IP běžeć (žórło je za Akamai Bot Manager — `curl` dóstanje 403), tohodla je laptop tak abo tak w hrajach.

**Kóšty:** kwalita přełožka je niša hač pola najlěpšeho modela. Za referencowy korpus, hdźež je originalny jendźelski tekst stajnje jenož kliknjenje zdaleny, je to w porjadku. Njetwjerdźimy, zo su přełožki awtoritatiwne.

### 3. Wobě połojcy dźělitaj so eksaktnje jedyn interfejs: wozjewjeny zwjazk

Pipeline ženje direktnje do produkciskeje datoweje banki njepisa. Wono wudawa `{ SQL, manifest zasobow, lisćina za wuprózdnjenje pufraka }`. "Wozjewjenje" = nałožić tutón zwjazk doprědka (posunyć SQL do edge SQL DB, synchronizować zasoby do objektoweho składowanišća, wuprózdnić mjenowane kluče pufraka).

**Čehodla:** lokalna strona a edge-strona móžetej so njewotwisnje wuwiwać; zwjazk je přehladujomny; a "zasadźić daty" ma kóždy raz tu samu formu. Worker je mała TypeScript/Hono-aplikacija — kruty CSP (nic `unsafe-inline`; inline JSON-LD je z sha256 připjaty), `Accept-Language` + kraj→rěčne jednanie, 30-dny KV-pufrak za strony, dnjowy cron za porjadowanje — a wón ženje njetrjeba wědźeć, kak su daty nastałe.

**Kóšty:** změna D1-šemy dótka dwě dataji (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Lacne zawěsćenje.

### Njediskutujomne zaležnosće, kotrež su do zadźerženja integrowane

- Njezwjazany z knježerstwom ZSA; žane oficielne znamjenja.
- Žórłowe redakcije so zdźerža, ženje so njewobroća.
- Widejo so DVIDS / AARO připisa.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na cyłym sydle — pytawko-indeksujomny, wotzjewjeny za AI-scraping.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
