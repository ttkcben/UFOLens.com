# GitHub — Póst 3 z 3 · Pśispomnjeśa k architekturje (diskusija w ADR-stilu)

**Wužyś:** ako diskusija pód "Pokaž a powědaj" / "Architektura" abo ako zakład za `docs/` ADR.
**Klucowe słowa:** architektura, ADR, forward-only state machine, lokalny LLM, Ollama, OCR, edge computing, CSP, wěstotne głowy, data pipeline, kóstowa inženjerija, SQLite manifest, D1, R2, KV
**Hyperwótkaze:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cogodla jo ufolens.com tak twórjony, ako jo

Pśispomnjeśa k tśom rozsuźenjam, kótarež su [ufolens.com](https://www.ufolens.com) (pśepytujobne, wěcejrěcne wózjawjenje [archiwa PURSUE UAP](https://www.war.gov/ufo)) formowali. Komentary / pśeśiwnosć su witane.

### 1. Pipeline jo na wěsći w forward-only state machine

Stawy: `discovered → downloaded → ocr_done → translated → published`. Dokument se jano doprědka pógibujo, a jano gaž jo źěło. Wózjawjony wopśimjeśe se nigda znowego njepśeźěłowa, snaźkuli delta-detektor wiźi, až se jo žrědło wopšawdu změniło.

**Cogodla:** OCR + pśełožowanje stej skóre operacije, a archiw rosćo z casom. Pipeline, kótaryž "wšykno znowego wuwjeźo, aby wěsty był", ma njewobgranicowane kószty. Z tym až se njemóžno cyni slědkpógibowanja, se njemóžno cyni njekontrolěrowana faktura. Kóńc kóstow jo kakosć statawego grafa, nic alertnosći operatora.

**Kószty:** migracije šemow a pśeźěłowanje na wěsći su narownje njewšedne. Pśizwólenja gódny kompromis.

### 2. OCR a pśełožowanje se na lokalnem LLM wuwjeźujotej, nic na cloud-API

OCR: open-source engine, Tesseract CLI fallback. Pśełožowanje + NER: Gemma pśez Ollama, na laptopje Apple Silicon.

**Cogodla:** nulowe marginalne kószty na dokument; reproducěrujobne (fiksn model + prompty); a fetch-schójźeńk musy južo z rezidencielnego IP wuwjeźony byś (žrědło jo za Akamai Bot Manager — `curl` dostanjo 403), togodla jo laptop tak abo tak w graśu.

**Kószty:** kwalita pśełožka jo pód frontowym modelom. Za referencny korpus, źož jo originalny engelšćina pśecej jano jadno kliknjenje daloko, jo to w pórěźe. Njetwjerźimy, až su pśełožki awtoritatiwne.

### 3. Dwě połojcy źělitej se eksaktnje jaden zwězowanje: wózjawjony paket

Pipeline nigda direktnje do produkcijskeje datoweje banki njepisa. Wón wudawa `{ SQL, asset manifest, cache-purge list }`. "Wózjawjenje" = nałoženje togo paketa doprědka (pósunjenje SQL do edge SQL DB, synchronizacija assetow do objektowego składowanja, wuproznjenje mjenjowanych cache-klucow).

**Cogodla:** lokalny bok a kšomowy bok móžotej se njewótwisnje dalej wuwijaś; paket jo pśeglědujobny; a "daty nasajźiś" jo kuždy raz tej samej formy. Worker jo mały TypeScript/Hono-app — strog CSP (žeden `unsafe-inline`; inline JSON-LD jo sha256-pśipěty), `Accept-Language` + kraj→rěc-dogronjenje, 30-dnjowy KV-cache za boki, wšedny cron za porěźenje — a wón nigda njemusy wěźeś, kak su daty nastałe.

**Kószty:** změna D1-šemy se tyka dweju datajowu (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Tani wěstota.

### Njewujadnajobne, do zachowanja zapackowane

- Njejo zwězany z kněžaŕstwom ZDA; žedne oficielne insignije.
- Žrědłowe redakcije se wobchowaju, nigda njewótwrośiju.
- Video se pśipisujo DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na cełem boku — indeksěrujobny za pytawy, wótzjawjony z AI-scrapinga.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
