# GitHub — Kannad 1 eus 3 · Embannadenn / Blok kemenn README

**Implij evel:** korf un Embannadenn GitHub, ur gaozenn benveket, pe e penn uhelañ ar README.
**Gerioù-alc'hwez:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hiperliammoù:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — ur savenn liesyezhek ha klaskadus evit dielloù PURSUE an UAP

**War-eeun:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Diell orin:** https://www.war.gov/ufo

`ufolens.com` a adembann dielloù **PURSUE** Departamant ar Brezel stadunanat a-zivout an enrolladennoù UAP / UFO didan-zifenn evel ur savenn anaoudegezh : klask en destenn a-bezh, troidigezh emgefreek a-dreuz ar c'horpus, ergerzhadenn kartenn + linnen-amzer, hag un API JSON foran. An teulioù orin a zo labourioù eus gouarnamant kevredadel ar Stadoù-Unanet hag e-barzh ar Stadoù-Unanet emaint er domani foran ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ar raktres-mañ **n'eo ket liammet gant gouarnamant ar Stadoù-Unanet**, ne implij merk ofisiel ebet, ha ne zizreol ket morse an adaozadennoù.

### Savouriezh

```
Milin lec'hel (Apple Silicon, IP annez)        Rouedad Edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, kalon stdlib-hepken)      worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (war-raok-hepken)    /{lang}/...   pajennoù
  OCR: keflusker open-source (distro Tesseract CLI)     /api/v1/...   API foran
  translate / NER: LLM lec'hel (Gemma dre Ollama)        /admin        konsol an oberatour
  stad: manifest SQLite                             harpet gant: diaz-titouroù SQL edge, stokadenn
        │                                              objedoù (PDFoù orin), grubuilh KV
        └── a embann ur pakad: SQL + manifest danvezioù + listenn da skarzhañ eus ar grubuilh ──┘
```

- **Koust cloud-AI mann dre zeul.** An OCR hag an droidigezh a ya en-dro war al lec'h ; ar mekanik stad war-raok-hepken (`discovered → downloaded → ocr_done → translated → published`) a warant n'eo adproseset teul ebet nemet ma vefe cheñchet.
- **Kalon ar Pipeline n'en deus tamm dépendandelezh tiers ebet** — ar moduloù parsing / manifest / delta a ya en-dro hag a vez amprouet war ur Python glan hep netra pip-staliet ; an tappennoù OCR/troidigezh a ziskenn en un doare dereat pa vez pakoù diret er-maez.
- **Al lec'hienn Edge** a laka talbennoù surentez strizh + CSP (hep `unsafe-inline`; an JSON-LD enlinenn zo `sha256`-benveket), marc'hataerezh yezh dre `Accept-Language` + kartennañ broioù, ur grubuilh pajennoù KV 30-deiz, hag un cron pemdeziek evit an derc'hel-ratre.
- **Hizivadurioù inkrementel:** un detektor delta a ziforc'h meneger an tarzh hag a voueta nemet ar c'hemmoù en-dro er pipeline.

### Evit an diorrenourien

An API foran e https://www.ufolens.com/api/v1 a zistro teulioù ha metaroadennoù e JSON. Bevennet eo ar moned dizanv ; goulennit un alc'hwez evit liveoù an enklaskerien/diorrenourien. Gwelit rann an API war al lec'hienn evit an termenelloù hag ar bevennoù.

### Stad

Kod klok ; lec'hienn dispaket e https://www.ufolens.com. An diaz-titouroù produiñ a zo poblet en ur lakaat ar pipeline ez-linenn da vont en-dro hag en ur embann ar pakad war-raok (`cli_publish run --remote`). An teulioù tresañ klok a zo e-barzh `docs/20260511/`.

### Aotre-implijout / bevennoù

- Teulioù orin: Labourioù eus gouarnamant kevredadel ar Stadoù-Unanet, domani foran e-barzh ar Stadoù-Unanet.
- Kod ar savenn-mañ hec'h-unan: gwelit `LICENSE`.
- Al lec'hienn a gas `Tdm-Reservation: 1` hag `X-Robots-Tag: noai, noimageai` — menegeradus gant ar c'hefluskerioù klask, dibabet er-maez eus gourdoniñ/skrapañ an AI.
- Ar videoioù a zo lakaet war anv DVIDS / AARO ha n'int ket arc'het gant ar raktres-mañ.

Degemeret mat eo an Issues hag ar PRs. Lennit `CLAUDE.md` ha `docs/20260511/00-*` a-raok digeriñ kemmoù strukturel.
