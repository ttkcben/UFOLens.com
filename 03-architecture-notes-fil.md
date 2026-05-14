# GitHub — Post 3 ng 3 · Mga tala sa arkitektura (ADR-style Discussion)

**Gamitin bilang:** isang Discussion sa ilalim ng "Show and tell" / "Architecture", o `docs/` ADR seed.
**Keywords:** arkitektura, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bakit ganito ang pagkagawa sa ufolens.com

Mga tala sa tatlong desisyon na humubog sa [ufolens.com](https://www.ufolens.com) (ang nahahanap at multilinggwal na muling pagtatayo ng [PURSUE UAP archive](https://www.war.gov/ufo)). Malugod na tinatanggap ang mga komento / pagtutol.

### 1. Ang pipeline ay isang forward-only state machine — sinasadya

Mga estado: `discovered → downloaded → ocr_done → translated → published`. Isang dokumento ay pasulong lamang gumagalaw, at kapag may kailangang gawin lamang. Ang nai-publish na nilalaman ay hindi kailanman muling pinoproseso maliban kung makita ng isang delta detector na nagbago talaga ang source.

**Bakit:** Ang OCR + translation ay ang mga mamahaling operasyon, at lumalaki ang archive sa paglipas ng panahon. Ang isang pipeline na "muling pinapatakbo ang lahat para makasiguro" ay may walang hangganang gastos. Ang paggawa ng mga paatras na transisyon na imposible ay ginagawang imposible ang isang runaway bill. Ang cost ceiling ay isang katangian ng state graph, hindi ng pagbabantay ng operator.

**Gastos:** ang mga schema migration at sadyang muling pagpoproseso ay sinasadyang awkward. Katanggap-tanggap na tradeoff.

### 2. Ang OCR at translation ay tumatakbo sa isang lokal na LLM, hindi sa isang cloud API

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma sa pamamagitan ng Ollama, sa isang Apple Silicon laptop.

**Bakit:** zero marginal cost bawat dokumento; reproducible (nakapirming modelo + mga prompt); at ang hakbang sa pag-fetch ay kailangan nang tumakbo mula sa isang residential IP (ang source ay nasa likod ng Akamai Bot Manager — ang `curl` ay nakakakuha ng 403), kaya isang laptop ay nasa loop na rin.

**Gastos:** ang kalidad ng pagsasalin ay mas mababa sa isang frontier model. Para sa isang reference corpus kung saan ang orihinal na Ingles ay laging isang click lang ang layo, ayos lang iyon. Hindi namin inaangkin na ang mga pagsasalin ay authoritative.

### 3. Ang dalawang hati ay nagbabahagi ng eksaktong isang interface: isang na-publish na bundle

Ang pipeline ay hindi kailanman direktang nagsusulat sa production database. Naglalabas ito ng `{ SQL, asset manifest, cache-purge list }`. Ang "Pag-publish" = ilapat ang bundle na iyon pasulong (itulak ang SQL sa edge SQL DB, i-sync ang mga asset sa object storage, i-purge ang mga pinangalanang cache key).

**Bakit:** ang lokal na panig at ang edge na panig ay maaaring magbago nang nakapag-iisa; ang bundle ay maaaring suriin; at ang "deploy data" ay pareho ang hugis sa bawat oras. Ang Worker ay isang maliit na TypeScript/Hono app — mahigpit na CSP (walang `unsafe-inline`; ang inline na JSON-LD ay sha256-pinned), `Accept-Language` + country→language negotiation, 30-araw na KV page cache, araw-araw na housekeeping cron — at hindi nito kailangang malaman kung paano ginawa ang data.

**Gastos:** ang isang pagbabago sa D1 schema ay nakakaapekto sa dalawang file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Murang insurance.

### Mga hindi-negosyableng nakapaloob sa pag-uugali

- Hindi kaakibat sa gobyerno ng U.S.; walang opisyal na sagisag.
- Ang mga source redaction ay pinapanatili, hindi kailanman binabaligtad.
- Ang video ay iniugnay sa DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` sa buong site — maaaring i-index ng search, naka-opt out sa AI-scrape.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

