# GitHub — 1. no 3 ierakstiem · Izlaiduma / README paziņojumu bloks

**Lietot kā:** GitHub laidiena pamattekstu, piespraustu diskusiju vai repozitorija README augšdaļu.
**Atslēgvārdi:** UAP, UFO, PURSUE arhīvs, deklasificēti dokumenti, atvērtie dati, pilna teksta meklēšana, OCR, mašīntulkošana, lokālais LLM, Ollama, malu skaitļošana, publiskais API, Hono, TypeScript, Python
**Hipersaites:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — daudzvalodu, meklējama platforma PURSUE UAP arhīvam

**Tiešraide:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Avota arhīvs:** https://www.war.gov/ufo

`ufolens.com` atkārtoti publicē ASV Kara departamenta **PURSUE** deklasificēto UAP / UFO ierakstu arhīvu kā zināšanu platformu: pilna teksta meklēšana, mašīntulkošana visā korpusā, kartes + laika skalas izpēte un publisks JSON API. Avota dokumenti ir ASV federālās valdības darbi un ASV teritorijā ir publiski pieejami ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Šis projekts **nav saistīts ar ASV valdību**, neizmanto oficiālas zīmotnes un nekad neatceļ redakcijas.

### Arhitektūra

```
Lokālā iekārta (Apple Silicon, mājsaimniecības IP)   Malu tīkls
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only kodols)         worker/  (TypeScript, Hono.js)
  iegūt → OCR → tulkot → publicēt  (tikai uz priekšu)  /{lang}/...   lapas
  OCR: atvērtā koda dzinējs (Tesseract CLI kā rezerve) /api/v1/...   publiskais API
  tulkošana / NER: lokālais LLM (Gemma caur Ollama)     /admin        operatora konsole
  stāvoklis: SQLite manifests                        nodrošina: malu SQL DB, objektu
        │                                              glabātuve (avota PDF), KV kešatmiņa
        └── publicē pakotni: SQL + resursu manifests + kešatmiņas tīrīšanas saraksts ──┘
```

- **Nulles mākoņa AI izmaksas par dokumentu.** OCR un tulkošana tiek veikta lokāli; tikai uz priekšu vērstā stāvokļa mašīna (`atklāts → lejupielādēts → ocr_pabeigts → tulkots → publicēts`) garantē, ka neviens dokuments netiek atkārtoti apstrādāts, ja vien tas nav mainījies.
- **Konveijera kodolam nav trešo pušu atkarību** — parsēšanas / manifesta / delta moduļi darbojas un tiek testēti uz tīra Python bez `pip` instalētām paketēm; OCR/tulkošanas posmi graciozi degradējas, ja trūkst izvēles pakotņu.
- **Malu vietne** piemēro stingras drošības galvenes + CSP (nav `unsafe-inline`; iekļautais JSON-LD ir piesaistīts ar sha256), valodu sarunas, izmantojot `Accept-Language` + valstu kartēšanu, 30 dienu KV lapu kešatmiņu un ikdienas uzturēšanas cron darbu.
- **Inkrementāli atjauninājumi:** delta detektors salīdzina avota indeksu un padod atpakaļ konveijerā tikai izmaiņas.

### Izstrādātājiem

Publiskais API, kas pieejams vietnē https://www.ufolens.com/api/v1, atgriež dokumentus un metadatus JSON formātā. Anonīmai piekļuvei ir ierobežots ātrums; pieprasiet atslēgu pētnieku/izstrādātāju līmeņiem. Skatiet vietnes API sadaļu, lai uzzinātu par galapunktiem un ierobežojumiem.

### Statuss

Kods ir pabeigts; vietne ir izvietota adresē https://www.ufolens.com. Produkcijas datubāze tiek aizpildīta, palaižot bezsaistes konveijeru un publicējot pakotni uz priekšu (`cli_publish run --remote`). Pilna dizaina dokumentācija atrodas `docs/20260511/`.

### Licence / robežas

- Avota dokumenti: ASV federālās valdības darbi, publiski pieejami ASV teritorijā.
- Šīs platformas kods: skatīt `LICENSE`.
- Vietne nosūta `Tdm-Reservation: 1` un `X-Robots-Tag: noai, noimageai` — indeksējama meklētājprogrammām, atteikusies no AI apmācības/skrāpēšanas.
- Video materiāli tiek attiecināti uz DVIDS / AARO, un šis projekts uz tiem nepretendē.

Problēmu ziņojumi un PR ir laipni gaidīti. Lūdzu, izlasiet `CLAUDE.md` un `docs/20260511/00-*` pirms strukturālu izmaiņu ierosināšanas.

