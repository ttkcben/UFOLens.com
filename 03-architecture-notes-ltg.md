# GitHub — 3. nu 3 paziņuojumim · Arhitektūras pīzeimes (ADR stila saruna)

**Lītuot, kai:** sarunu tematu sadaļā "Rōdīt un stōstīt" / "Arhitektūra" voi `docs/` ADR ōlūtu.
**Atslāgvārdi:** arhitektūra, ADR, tikai uz prīkšu vērsta stōvūkļa mašīna, vītejais LLM, Ollama, OCR, malu skaitļōšona, CSP, drūšības galvenes, datu cauruļvads, izmoksu inženierija, SQLite manifests, D1, R2, KV
**Hipersaites:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Deļkuo ufolens.com ir izveidōts tai, kai tys ir

Pīzeimes par trejim lēmumim, kas veidōja [ufolens.com](https://www.ufolens.com) (meklējamo, daudzvalōdeigo [PURSUE UAP arhiva](https://www.war.gov/ufo) atjaunōjumu). Komentāri / kritika ir gaideiti.

### 1. Cauruļvads ir apzynāti tikai uz prīkšu vērsta stōvūkļa mašīna

Stōvūkļi: `atklōts → lejupīlōdēts → ocr_pabeigts → iztulkōts → publicēts`. Dokuments pōrvietojās tikai uz prīkšu un tikai tod, kod ir jōveic dorbs. Publicēts saturs nikod netiek atkōrtōti apstrōdōts, izjemūt, jo delta detektors pamona, ka ōlūts ir patīšom mainējīs.

**Deļkuo:** OCR + tulkōšona ir dōrgōkōs operācijas, un arhivs ar laiku aug. Cauruļvodam, kas "atkōrtōti palaiž vysu, lai byutu drūši", ir naīrūbežōtas izmaksas. Padareit atpakaļejošas pōrejas naīspējamas, padora naīspējamu ari nekontrolējamu rēķinu. Izmoksu grīsti ir stōvūkļa grafa, na operatora modruma īpašība.

**Izmaksas:** shēmas migrācijas un apzynāta atkōrtōta apstrōde ir tīšom naērtas. Pījemams kompromiss.

### 2. OCR un tulkōšona dorbojās ar vīteju LLM, na muokūņa API

OCR: atvārtō ōlūta dzynējs, Tesseract CLI rezervis. Tulkōšona + NER: Gemma caur Ollama, uz Apple Silicon klēpjdatora.

**Deļkuo:** nulle marginālōs izmaksas par vīnu dokumentu; reproducējams (fiksēts modelis + uzdevumi); un iegōdes solis jau ir jōpalaiž nu dzeivojamōs vītas IP (ōlūts ir aiz Akamai Bot Manager — `curl` sajem 403), tōpēc klēpjdators ir procesā jau taipat.

**Izmaksas:** tulkōjuma kvalitāte ir zemōka nekā mūsdienu modeļim. Atsauces korpusam, kur oriģinālais angļu teksts vienmār ir vīna klikšķa attōlumā, tys ir pīņemami. Mēs naapgalvojom, ka tulkōjumi ir autoritatīvi.

### 3. Abas puses dala vīnu saskarni: publicētu paketi

Cauruļvads nikod naroksta ražōšonas datubāzē tīši. Tys izdūd `{ SQL, līdzekļu manifests, kešatmiņas tīrīšonas saroksts }`. "Publicēšona" = pīlītojit šū paketi uz prīkšu (pabīdit SQL uz malu SQL DB, sinhronizējit līdzekļus ar objektu krōtuvi, iztīrit nūsauktōs kešatmiņas atslāgas).

**Deļkuo:** vītejō puse un malu puse var attīstīties neatkarīgi; pakete ir pōrbaudāma; un "datu publicēšona" ir vīnmār vīnā formā. Worker ir neliela TypeScript/Hono aplikācija — stingrs CSP (nav `unsafe-inline`; ielaistais JSON-LD ir sha256-pīsprausts), `Accept-Language` + valsts→valōdas saskaņōšona, 30 dīnu KV lopu kešatmiņa, ikdīnas uzturēšonas cron uzdevums — un tai nikod nav jōzynoj, kai dati tika izveidōti.

**Izmaksas:** D1 shēmas izmaiņa skar divus failus (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Lāta apdrūšynōšona.

### Naapspriežami nūsacejumi, kas īstrōdōti uzvedībā

- Nav saisteits ar ASV valdību; nav oficiālu atpazeišonas zīmju.
- Ōlūta redakcijas tiek saglobōtas, nikod neatceļtas.
- Video tiek attiecināti uz DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` visā vītnē — meklētājprogrammām indeksējams, AI datu izvylkšona ir atteikta.

Dzeivajā: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

