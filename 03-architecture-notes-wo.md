# GitHub — Post 3 of 3 · Architecture notes (ADR-style Discussion)

**Use as:** a Discussion under "Show and tell" / "Architecture", or `docs/` ADR seed.
**Keywords:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Lu tax ñu tabaxee ufolens.com ni

Ay kaddu ci ñetti desisyon yu defar [ufolens.com](https://www.ufolens.com) (defaraat bu mën a seet te am ay làkk yu bare bu dalu [PURSUE UAP archive](https://www.war.gov/ufo)). Amul solo su ngeen amee ay komaŋteer / kritik.

### 1. Pipeline bi ab nosteg jëm-kanam rekk la — ci campañ

Eta: `discovered → downloaded → ocr_done → translated → published`. Ab mbind dafay jëm kanam rekk, te su amee liggéey lu mu war a def rekk. Mbind mu ñu pibli duñu ko defaraat lu dul su ab gistar delta gisee ne cosaan bi dafa soppiku.

**Lu tax:** OCR + tekki ñooy operasyon yi seer, te dal bi dafay màg ak jamono. Ab pipeline buy "defaraat lépp ngir wóor" amul benn àtte bu njëg. Su ñu tee jëm ginnaaw, kon faktir bu réy du mën a am. Àtte bu njëg bi ci grafu eta bi la nekk, nekkul ci saytug operëer bi.

**Njëg:** migrasyonu schema ak defaraat-ci-campañ dañu jafe-jafe lool. Ab coobare bu ñu nangu la.

### 2. OCR ak tekki dañuy dox ci ab LLM bu nekk ci ordinatëer, du ci ab API cloud

OCR: moteur bu ubbiku, Tesseract CLI fallback. Tekki + NER: Gemma via Ollama, ci ab laptop Apple Silicon.

**Lu tax:** njëguñu dara ci mbind bu nekk; mën nañu ko defaraat (model + prompts yu taxaw); te etapu jëli bi dafa war a dox ci ab IP kër (cosaan bi nekk na ginnaaw Akamai Bot Manager — `curl` dafay joxe 403), kon laptop bi ci biir loop bi la nekk.

**Njëg:** kalite tekki bi nekk na suufu model bu gën a fës. Ngir ab dalu referans fu angale cosaan bi nekk ci benn klik rekk, loolu baax na. Waxuñu ne tekki yi dañoo wóor.

### 3. Ñaari xaaj yi dañoo bokk benn jongo rekk: ab paket bu ñu pibli

Pipeline bi du bind mukk ci base de données bu prod bi. Dafay génne `{ SQL, asset manifest, cache-purge list }`. "Pibli" = jëlee paket boobu (dugal SQL ci base de données SQL bu edge, sinkronise resurs yi ci stokas obse, génne caabi kas yu ñu tuddu).

**Lu tax:** xaaju ordinatëer bi ak xaaju edge bi mën nañoo evolye ci seen bopp; paket bi mën nañu ko xool; te "deploy data" dafa am benn melokaan bu sax. Worker bi ab appu TypeScript/Hono bu tuut la — CSP bu dëgër (amul `unsafe-inline`; inline JSON-LD dafa `sha256`-pinned), `Accept-Language` + waxtaanu réew→làkk, kasu xët bu 30 fan ci KV, cron buy settal bés bu nekk — te soppiwul mukk xam nan lañu defaree data bi.

**Njëg:** ab soppi ci schema D1 dafay laal ñaari fisiye (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Asurans bu yomb.

### Yu ñu mënul a waxtaane ci doxalin bi

- Bokkul ci ngornamaŋ U.S.; amul mandarga bu ofisel.
- Li ñu nëbb ci cosaan bi dañu koy sàmm, duñu ko dindi mukk.
- Wideo yi dañu leen jox DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ci dal bi yépp — mën nañu ko indexe ci seetukaay, waaye génn na ci njàngum AI/scrape.

Ci Kanam: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

