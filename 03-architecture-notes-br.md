# GitHub — Kannad 3 eus 3 · Notennoù savouriezh (Kaozeadenn doare-ADR)

**Implij evel:** ur gaozenn dindan "Diskouez ha kontañ" / "Savouriezh", pe hadenn ADR evit `docs/`.
**Gerioù-alc'hwez:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hiperliammoù:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Perak e vez savet ufolens.com evel m'emañ

Notennoù diwar-benn an tri diviz o deus stummet [ufolens.com](https://www.ufolens.com) (an adsavadenn glaskadus ha liesyezhek eus [dielloù PURSUE an UAP](https://www.war.gov/ufo)). Degemeret mat eo an evezhiadennoù / enebiezh.

### 1. Ur mekanik stad war-raok-hepken eo ar pipeline — a-ratozh

Stadoù: `discovered → downloaded → ocr_done → translated → published`. Un teul ne fiñv nemet war-raok, ha nemet pa vez labour d'ober. An endalc'h embannet n'eo morse adproseset nemet ma wel un detektor delta ez eo cheñchet an tarzh e gwirionez.

**Perak:** An OCR + an droidigezh eo an oberiadurioù ker, hag an diell a gresk a-hed an amzer. Ur pipeline a "adlaka pep tra da vont en-dro evit bezañ sur" en deus ur c'houst didermen. Ober treuzchoareadennoù war-gil amposibl a ra ur fakturenn drollet amposibl. Lein ar c'houst a zo ur perzh eus graf ar stad, n'eo ket eus evezh an oberatour.

**Koust:** an eiladurioù skema hag an adprosesiñ a-ratozh a zo ameeun a-youl-gaer. Un eskemm degemerus.

### 2. An OCR hag an droidigezh a ya en-dro war un LLM lec'hel, n'eo ket un API cloud

OCR: keflusker open-source, distro Tesseract CLI. Troidigezh + NER: Gemma dre Ollama, war ul laptop Apple Silicon.

**Perak:** koust marzhel mann dre zeul ; adproduadus (model + prompterioù fiks) ; hag ar pazenn 'fetch' a rank dija mont en-dro adalek un IP annez (an tarzh a zo a-dreñv Akamai Bot Manager — `curl` a resev ur 403), setu ul laptop a zo er c'helc'h forzh penaos.

**Koust:** perzh an droidigezh a zo dindan ur model talar. Evit ur c'horpus dave e-lec'h m'emañ ar saozneg orin atav ur c'hlik pelloc'h, mat eo. Ne lavarom ket ez eo an troidigezhioù aotreet.

### 3. An daou hanterenn a rann un etrefas hepken : ur pakad embannet

Ar pipeline ne skriv morse war-eeun en diaz-titouroù produiñ. Embann a ra `{ SQL, manifest an danvezioù, listenn ar grubañchoù da skarzhañ }`. "Embann" = lakaat ar pakad-se war-raok (bountañ an SQL d'an diaz-titouroù SQL edge, sinkronizañ an danvezioù gant ar stokadenn objedoù, skarzhañ an alc'hwezioù grubuilh anvet).

**Perak:** an tu lec'hel hag an tu edge a c'hall emdreiñ en un doare dizalc'h ; ar pakad a c'hall bezañ adwelet ; ha "dispakañ roadennoù" en deus ar memes stumm bep tro. Ar Worker a zo un app bihan TypeScript/Hono — CSP strizh (hep `unsafe-inline`; an JSON-LD enlinenn zo `sha256`-benveket), `Accept-Language` + marc'hataerezh bro→yezh, ur grubuilh pajennoù KV 30-deiz, un cron pemdeziek evit an derc'hel-ratre — ha n'en deus morse ezhomm da c'houzout penaos eo bet graet ar roadennoù.

**Koust:** ur c'hemm e skema D1 a stok ouzh daou restr (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un asurañs marc'had-mat.

### Divarvarc'hadus e-barzh an emzalc'h

- N'eo ket liammet gant gouarnamant ar Stadoù-Unanet ; merk ofisiel ebet.
- Miret eo adaozadennoù an tarzh, morse dizreoliet.
- Video lakaet war anv DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` war an holl lec'hienn — menegeradus evit ar c'hlask, dibabet er-maez eus ar skrapañ AI.

War-eeun: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
