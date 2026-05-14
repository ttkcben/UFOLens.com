# GitHub — 3. no 3 ierakstiem · Arhitektūras piezīmes (ADR stila diskusija)

**Lietot kā:** diskusiju sadaļā "Parādīt un pastāstīt" / "Arhitektūra" vai `docs/` ADR sākumpunktu.
**Atslēgvārdi:** arhitektūra, ADR, tikai uz priekšu vērsta stāvokļa mašīna, lokālais LLM, Ollama, OCR, malu skaitļošana, CSP, drošības galvenes, datu konveijers, izmaksu inženierija, SQLite manifests, D1, R2, KV
**Hipersaites:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kāpēc ufolens.com ir veidots tieši šādi

Piezīmes par trim lēmumiem, kas veidoja [ufolens.com](https://www.ufolens.com) (meklējams, daudzvalodu [PURSUE UAP arhīva](https://www.war.gov/ufo) atjaunojums). Komentāri / iebildumi ir laipni gaidīti.

### 1. Konveijers ir tikai uz priekšu vērsta stāvokļa mašīna — ar nodomu

Stāvokļi: `atklāts → lejupielādēts → ocr_pabeigts → tulkots → publicēts`. Dokuments virzās tikai uz priekšu un tikai tad, ja ir darbs, kas jāveic. Publicēts saturs nekad netiek atkārtoti apstrādāts, ja vien delta detektors neredz, ka avots ir faktiski mainījies.

**Kāpēc:** OCR + tulkošana ir dārgākās operācijas, un arhīvs laika gaitā aug. Konveijeram, kas "pārlaiž visu no jauna drošības pēc", ir neierobežotas izmaksas. Padarot atpakaļejošas pārejas neiespējamas, tiek novērsta nekontrolējama rēķina iespējamība. Izmaksu griesti ir stāvokļa grafa īpašība, nevis operatora modrības rezultāts.

**Izmaksas:** shēmas migrācijas un apzināta atkārtota apstrāde ir tīši apgrūtinātas. Pieņemams kompromiss.

### 2. OCR un tulkošana tiek veikta ar lokālu LLM, nevis mākoņa API

OCR: atvērtā koda dzinējs, Tesseract CLI kā rezerve. Tulkošana + NER: Gemma, izmantojot Ollama, uz Apple Silicon klēpjdatora.

**Kāpēc:** nulles robežizmaksas par dokumentu; reproducējams (fiksēts modelis + uzdevumi); un iegūšanas solim jau tāpat ir jādarbojas no mājsaimniecības IP (avots ir aiz Akamai Bot Manager — `curl` saņem 403), tāpēc klēpjdators jebkurā gadījumā ir iesaistīts procesā.

**Izmaksas:** tulkojuma kvalitāte ir zemāka nekā jaunākajiem modeļiem. Atsauču korpusam, kur oriģinālais angļu teksts vienmēr ir viena klikšķa attālumā, tas ir pieņemami. Mēs neapgalvojam, ka tulkojumi ir autoritatīvi.

### 3. Abām pusēm ir tieši viena kopīga saskarne: publicēta pakotne

Konveijers nekad neraksta tieši produkcijas datubāzē. Tas izvada `{ SQL, resursu manifests, kešatmiņas tīrīšanas saraksts }`. "Publicēšana" = piemērot šo pakotni uz priekšu (nosūtīt SQL uz malu SQL DB, sinhronizēt resursus ar objektu glabātuvi, iztīrīt nosauktās kešatmiņas atslēgas).

**Kāpēc:** lokālā puse un malu puse var attīstīties neatkarīgi; pakotne ir pārskatāma; un "datu izvietošana" katru reizi ir vienādā formā. Worker ir neliela TypeScript/Hono lietotne — stingrs CSP (nav `unsafe-inline`; iekļautais JSON-LD ir piesaistīts ar sha256), `Accept-Language` + valsts→valodas sarunas, 30 dienu KV lapu kešatmiņa, ikdienas uzturēšanas cron darbs — un tai nekad nav jāzina, kā dati tika radīti.

**Izmaksas:** D1 shēmas izmaiņas skar divus failus (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Lēta drošības garantija.

### Neapspriežami principi, kas iestrādāti uzvedībā

- Nav saistīts ar ASV valdību; nav oficiālu zīmotņu.
- Avota redakcijas tiek saglabātas, nekad netiek atceltas.
- Video attiecināts uz DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` visā vietnē — meklētājprogrammām indeksējams, atteicies no AI skrāpēšanas.

Tiešraide: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

