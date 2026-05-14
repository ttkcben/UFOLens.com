# GitHub — Båatsoe 3 3:este · Arkitektuvre-notath (ADR-steele Diskusjovne)

**Nuhtjh goh:** Diskusjovne "Show and tell" / "Architecture" sisnie, jallh `docs/` ADR-seeds.
**Sleatkoe-baakoeh:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyper-laangh:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Man åvteste ufolens.com lea biggeme naemhtie

Notath golme sjæjsmah åvteste gïeh hammoedamme [ufolens.com](https://www.ufolens.com) (dïhte ohtsemes, gellie-gïeleldh dïsve-biggeme [PURSUE UAP-arkijvese](https://www.war.gov/ufo)). Kommentaarh / vuastalasse leah buere-baateme.

### 1. Pipeline lea voeride-guvvie staatemaasjine — meedie sjæjsmose

Staath: `discovered → downloaded → ocr_done → translated → published`. Dokumeente ajve voeride guvvie guhkede, jïh ajve gosse lea barkoe goh barkoedh. Publiseereme sisnesne ij gænnah dïsve-gïehtjedalleme jis ij delta-detektore seamma valke lea aktan jeatjehtamme.

**Man åvteste:** OCR + jarkoestimmie leah divves operasjonh, jïh arkijve jarkan over aejkie. Pipeline gïeh "dïsve-jåhteme gaajhkem åvteste jååhkes" lea raastemeles kåaste. Gosse gïehtjedalleme lea bïhkedamme baaktoe-guvvie, dellie runaway bill lea bïhkedamme. Kåaste-lååpome lea staate-graafen jïh ij operadöören vaere-vaarjelimmie.

**Kåaste:** skema-migrasjovnh jïh gïehtjedalleme-meht-sjæjsmoe leah meedie sjæjsmose geerve. Akseptabel bytte.

### 2. OCR jïh jarkoestimmie gïehtjedalleminie voenges LLM'isnie, ij pilve-API'sne

OCR: reahkes-valke mootore, Tesseract CLI baalhkh. Jarkoestimmie + NER: Gemma Ollama'n tjirrh, Apple Silicon-laptop'isnie.

**Man åvteste:** nulle marginal-kåaste per dokumeente; reproduceremes (vïedtjestamme modelle + prompth); jïh fetch-staale byøroe jåhteme voenges IP'ste (valke lea Akamai Bot Manager'n duakan — `curl` oahpselinie 403), dellie laptop lea loop'isnie jïh.

**Kåaste:** jarkoestimmie-kvaliteete lea vuollesne raastem-modelle. Referanse-korpus'se gusnie originaale engelske lea ajve akte klikk borte, dïhte lea buere. Ijieh jiehtieh jarkoestimmieh leah autoritative.

### 3. Dah göökte bielieh ajve akte interface juekieh: publiseereme bundle

Pipeline ij gænnah produrings-daatebaasebe gïetedidh. Dïhte seedeminie `{ SQL, asset manifest, cache-dåårjeme-lijste }`. "Publiseereminie" = daate bundle voeride-guvvie nuhtjedh (push SQL kant-SQL-DB'se, synke assets objekt-laahkese, dåårjeme neamhteme cache-keys).

**Man åvteste:** voenges bielie jïh kant-bielie maehtieh byögkeles bïhkedidh; bundle lea reviewemes; jïh "deploy data" lea seamma hammoe fïere gïjre. Worker lea onne TypeScript/Hono-app — gïerve CSP (ij `unsafe-inline`; inline JSON-LD lea sha256-vïedtjestamme), `Accept-Language` + laante→gïele-negotiasjovne, 30-daagijh KV-sæjroe-cache, fïere-bæjjien gåetie-ryddjemekron — jïh ij gænnah daajhtedh guktie daate lea sjïehtesjovveme.

**Kåaste:** D1-skema-jeatjeme gïehtjedalleminie göökte-filh (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Bïelje forsikring.

### Ij-diskuteremes byjjesbiehkieh

- Ij lahtestamme U.S.en reeremasse; ij voeremhke mïerkh.
- Valke-redaksjovnh gorresuvvieh, ij gænnah baalhkh.
- Video attribuert DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` sæjroe-vïedtjestamme — ohtsemes-indekseremes, AI-skraping-opt-out.

Jielije: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
