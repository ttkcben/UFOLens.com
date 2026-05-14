# GitHub — 1. nu 3 paziņuojumim · Izlaiduma / README paziņuojuma bloks

**Lītuot, kai:** GitHub izlaiduma pamatteksts, pīsprausts sarunu temats voi repozitorija README augšdaļa.
**Atslāgvārdi:** UAP, UFO, PURSUE arhivi, atslepenōti dokumenti, atvērtī dati, pilna teksta meklēšona, OCR, mašīntlulkōšona, vītejais LLM, Ollama, malu skaitļōšona, publiskais API, Hono, TypeScript, Python
**Hipersaites:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — daudzvalōdeiga, meklējama platforma PURSUE UAP arhivam

**Dzeivajā:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Ōlūta arhivs:** https://www.war.gov/ufo

`ufolens.com` atkārtōti publicē ASV Kara departamenta **PURSUE** atslepenōtō UAP / UFO īrakstu arhivu kai zynōšonu platformu: pilna teksta meklēšona, mašīntlulkōšona visā korpusā, kartes + laika skalas izpēte un publisks JSON API. Ōlūta dokumenti ir ASV federālōs valdības dorbs, un ASV teritōrijā tī ir publiski pīejami ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Šis projekts **nav saisteits ar ASV valdību**, nalītoj oficiālas atpazeišonas zīmes un nikod neatceļ redakcijas.

### Arhitektūra

```
Vītejais dators (Apple Silicon, dzeivojamōs vītas IP)   Malu teikls
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only kodols)         worker/  (TypeScript, Hono.js)
  iegōt → OCR → tulkōt → publicēt (tikai uz prīkšu)    /{lang}/...   lopas
  OCR: atvārtō ōlūta dzynējs (Tesseract CLI rezervis)   /api/v1/...   publiskais API
  tulkōt / NER: vītejais LLM (Gemma caur Ollama)       /admin        operatora konsole
  stōvūklis: SQLite manifests                        ar atbolstu: malu SQL DB, objektu
        │                                              krōtuve (ōlūta PDF), KV kešatmiņa
        └── publicē paketi: SQL + līdzekļu manifests + kešatmiņas tīrīšonas saroksts ──┘
```

- **Nulle izmoksu par vīnu dokumentu, kas saisteitas ar muokūņa AI.** OCR un tulkōšona nūteik vītejā datorā; tikai uz prīkšu vērstō stōvūkļa mašīna (`atklōts → lejupīlōdēts → ocr_pabeigts → iztulkōts → publicēts`) garantej, ka nivīns dokuments natiks atkōrtōti apstrōdōts, izjemūt, jo tys ir mainējīs.
- **Datu cauruļvada kodolam nav trešōm pusēm pīdarūšu atkarību** — parsēšonas / manifestu / delta moduļi dorbojās un tiek testēti tīrā Python vidē, kurā nav instalēts nikas ar pip; OCR/tulkōšonas posmi graciozi degradējās, kod opcionalōs pakotnes nav pīejamas.
- **Malu vīta** pīlītoj stingras drūšības galvenes + CSP (nav `unsafe-inline`; ielaistais JSON-LD ir pīsprausts ar sha256), valōdas saskaņōšonu, izmontojūt `Accept-Language` + valsts kartēšonu, 30 dīnu KV lopu kešatmiņu un ikdīnas uzturēšonas cron uzdevumu.
- **Inkrementāli atjaunynōjumi:** delta detektors atzeimē atškireibas ōlūta indeksā un padūd atpakaļ cauruļvodā tikai izmaiņas.

### Izstrōdōtōjim

Publiskais API, kas pīejams https://www.ufolens.com/api/v1, atgrīž dokumentus un metadatus JSON formatā. Anonīma pīkļuve ir īrūbežōta; pīpraseit atslāgu pētnīku/izstrōdōtōju līminim. Vairōk informācijas par API galapunktim un limitim skatit vītnes API sadalā.

### Statuss

Kods ir pabeigts; vītne ir publicēta https://www.ufolens.com. Ražōšonas datubāze tiek pīpildēta, palaižūt bezsaistes cauruļvadu un publicējūt paketi uz prīkšu (`cli_publish run --remote`). Pilna dizaina dokumentācija atrodama `docs/20260511/`.

### Licence / Rūbežas

- Ōlūta dokumenti: ASV federālōs valdības dorbi, publiski pīejami ASV teritōrijā.
- Šōs platformas kods: skatit `LICENSE`.
- Vītne sūta `Tdm-Reservation: 1` un `X-Robots-Tag: noai, noimageai` — meklētājprogrammas var indeksēt, bet ir atteikts nu AI apmōceibas/datu izvylkšonas.
- Video materiāli tiek attiecināti uz DVIDS / AARO, un šis projekts tūs naīprasa.

Problēmu pīteikumi un PR ir gaideiti. Lyudzu, izlasit `CLAUDE.md` un `docs/20260511/00-*` pyrms veidojat strukturālas izmaiņas.

