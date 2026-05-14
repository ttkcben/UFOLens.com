# GitHub — Poostâ 3/3 · Ruttâdâh-merkkimijd (ADR-stiijlâ savâstâllâm)

**Kevttim:** Savâstâlmân "Čäitim & muštâlem" / "Ruttâdâh" -vuálá, teikâ `docs/` ADR-säiđun.
**Suárkisanah:** ruttâdâh, ADR, ovdâskulkee tilá-mašin, páihálâš LLM, Ollama, OCR, roovdiiräijih-lâšettem, CSP, tiervâsvuotâ-koodih, data-putkâ, kolo-inženööri, SQLite-manifest, D1, R2, KV
**Hyperliŋkah:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Manen ufolens.com lii huksejum návt

Merkkimijd kuulmâ miärádâsâst, moh hämmejeh [ufolens.com](https://www.ufolens.com) (uccâmnáál, maaŋgâkielâlâš uđđâsist huksim [PURSUE UAP -arkkâdâh](https://www.war.gov/ufo)). Kommentih / vuástápuáttim tiervâpuáttim.

### 1. Putkâ lii ovdâskulkee tilá-mašin — tarkkuutuksáin

Tileh: `kávnum → luđidum → ocr_valmâš → jurgâlum → almostittum`. Dokument joođah tuš ovdâskulij, já tuš ko lii porgâmnáál. Almostittum siskáldâs ij kuássin keevât uđđâsist, jis ij delta-tievdâttâh uáin, et algâalgâttâh lii tuođâi nubásmum.

**Manen:** OCR + kiärkkäläs láá tivrrâs operaatioh, já arkkâdâh šadda ääigi mield. Putkâ, mii "jotât puoh uđđâsist tiervâsvuođâ tiet" lii raijihis kolo. Ko maasâd-kulkee sajattuvah láá västidemnááh, te tot västid raijihis reekki. Koloi alarääji lii tilá-graafi oomâšvuotâ, ij hoittájeijee vâruttâs.

**Kolo:** schema-migraatioh já tarkkuutuksáin uđđâsist keevâttem láá tarkkuutuksáin suojâliih. Tuhtâleijee vääridem.

### 2. OCR já kiärkkäläs joteh páihálâš LLM:in, ij pilvâ-API:in

OCR: ávus koodi moottor, Tesseract CLI varan. Kiärkkäläs + NER: Gemma Ollama pehti, Apple Silicon laaptopist.

**Manen:** nullâ lisikolo dokument kuáhtás; uđđâsist porgâmnáál (kiŋŋituttum model + piiđoseh); já vieččâm-mudo kalga jo jotteeđ ässee-IP:st (algâalgâttâh lii Akamai Bot Manager tuáhin — `curl` finnee 403), nuuvt et laaptop lii jo loopist.

**Kolo:** kiärkkäläs-kvaliteet lii vuolleebeht ko roovdiiräiji-model. Referens-korpusân, kost algâalgâttâh lii ain oovtâ klikkâlem keežin, tot lii pyere. Mij iä kiptâ, et kiärkkäläsah liččii autoritatiivliih.

### 3. Kyevti peelni lii täälh oovtâ interfeis: almostittum pookkijd

Putkâ ij kuássin čääli njuolgist tuotâdvuotâ-tietokantaan. Tot puáhtá `{ SQL, oomeet-manifest, muštolâš-tihheem-listo }`. "Almostittem" = keevt ton pookkijd ovdâskulij (tooimât SQL roovdiiräiji SQL DB:n, synkronisere oomeetijd objekt-vuárkkán, tihhee nommâstum muštolâš-čuovgijd).

**Manen:** páihálâš peeli já roovdiiräiji-peeli pyehtih ovdániđ jiešlottehen; pookkijd lii árvuštâllâmnáál; já "vuolgât data" lii ain siämmáá háámás. Worker lii uce TypeScript/Hono-app — čuolmâd CSP (ij `unsafe-inline`; inline JSON-LD lii sha256-kiŋŋituttum), `Accept-Language` + eennâm→kielâ-šopâmuš, 30-peeivi KV-siijđomuštolâš, jyehip peeivi huolâtâs-cron — já tot ij kuássin taarbâš tiettiđ, maht data lii rahtum.

**Kolo:** D1-schema-nubástus kyeskâ kyehti tiätturáid (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Hälbis täähidem.

### Ij-šopâmnáál tuáimeh, moh láá láhtteemist

- Ij lah kulâlâš Ovtâstum väldikodij haldâttâhân; iä almâlâš tubdâlduvah.
- Algâalgâttuv čapisvuotâh seilujeh, iä kuássin jurâsuvvo.
- Video lii koledum DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` siijđo oles — uuccâm-indekseerdemnáál, AI-ruápámist-iiváldum.

Ijoo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
