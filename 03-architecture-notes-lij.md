# GitHub — Post 3 de 3 · Nòtte de architetûa (Discusción in stîle ADR)

**Dêuvo:** cómme 'na Discusción sótta "Móstra e cónta" / "Architetûa", ò cómme spónto pe 'n ADR in `docs/`.
**Paròlle ciave:** architetûa, ADR, màchina a stâti solo in avanti, LLM locâle, Ollama, OCR, edge computing, CSP, header de seguéssa, pipeline de dæti, ingegnerîa di còsti, manifesto SQLite, D1, R2, KV
**Colegamenti ipertestoâli:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Perché ufolens.com o l'é constrûto cómme o l'é

Nòtte in scê træ decixoìn che àn dæto forma a [ufolens.com](https://www.ufolens.com) (a ricostruçión riçercàbile e multilingua de l'[archivio PURSUE UAP](https://www.war.gov/ufo)). Coménti e crìtiche són benvegnûi.

### 1. O pipeline o l'é 'na màchina a stâti solo in avanti — apropòsta

Stâti: `descoerto → descaregòu → ocr_fæto → tradûto → publicòu`. 'N documento o se sposta solo in avanti, e solo quànde gh'é da travagiâ. O contegnûo publicòu o no vên mâi tórna elaboròu a mêno che 'n rilevatô de delta o no védde che a vivàgna a l'é pe da bón cangiâ.

**Perché:** OCR e traduçión són e operaçioìn ciù câre, e l'archivio o crésce co-o ténpo. 'N pipeline che "o tórna a fâ tutto pe segurêssa" o l'à 'n còsto sénsa lìmite. Rende inposcìbili e tranxiçioìn a l'inderrê o rénde inposcìbile 'na bòlla sénsa contròllo. O tetto màscimo de spéiza o l'é 'na propietæ do gràfo di stâti, no da vigilànza de l'operatô.

**Còsto:** e migraçioìn de schêma e a ri-elaboraçión apropòsta són deliberataménte sgrêuzie. 'N conpromìsso acetàbile.

### 2. OCR e traduçión gîan in sce 'n LLM locâle, no 'n'API cloud

OCR: motô open-source, fallback Tesseract CLI. Traduçión e NER: Gemma via Ollama, in sce 'n laptop Apple Silicon.

**Perché:** còsto marginâle zero pe documento; riproducìbile (modéllo e prompt fìsci); e a fâze de acàtto a dêve za gjâ da 'n IP rescidençiâle (a vivàgna a l'é derê a Akamai Bot Manager — `curl` o l'òtén 'n 403), dónca 'n laptop o l'é comùnque into cìrcolo.

**Còsto:** a qualitæ da traduçión a l'é ciù bàssa de 'n modéllo a-a frontêa. Pe 'n corpus de riferiménto dónde l'originâle ingléize o l'é sénpre a 'n clìcche de distànsa, quésto o va bén. No pretendiàn che-e traduçioìn séggian outorévole.

### 3. E doê meitæ condivìddan sôlo un'interfàccia: 'n pachetto publicòu

O pipeline o no scrîve mâi diretaménte into database de produçión. O prodûce `{ SQL, manifesto di asset, lista de purgaçión da cache }`. "Publicâ" = aplicâ quéllo pachetto in avanti (spìnze l'SQL a-o DB SQL a-o bòrdo, sincronizâ i asset a l'archiviaçión de ògètti, purgâ e ciâve de cache nominæ).

**Perché:** a pàrte locâle e a pàrte a-o bòrdo pêuan evolvéscise in maniêra indipendénte; o pachetto o l'é revixonàbile; e "distriboî i dæti" o l'é 'n procèsso sénpre co-a mæxima fórma. O Worker o l'é 'n'aplicaçión picìnn-a in TypeScript/Hono — CSP strétto (nisciùn `unsafe-inline`; i JSON-LD inlìnia són fisæ con sha256), negociaçión `Accept-Language` + pàize→léngoa, cache de pàgina KV de 30 giórni, cron de manutençión giornaliêro — e o no l'à mâi de bezéugno de savéi cómme i dæti són stæti creæ.

**Còsto:** 'n cangiaménto a-o schêma D1 o tócca doî file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). 'N'asicurasiôn ch'a no cósta nìnte.

### Elemenénti no-negociàbili integræ into conportaménto

- No afiliòu a-o govèrno di Stati Unîi; nisciùn insìgna ofiçiâ.
- E redaçioìn da vivàgna són conservæ, mâi reversæ.
- I filmâti són atriboîi a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` pe tutto o scîto — indicisàbile da-i motoî de riçerca, disattivòu da-o scraping de AI.

In dirètta: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

