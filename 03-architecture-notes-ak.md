# GitHub — Nkrataa 3 a ɛwɔ 3 mu · Nhyehyɛeɛ ho nkrata (ADR-style Discussion)

**Fa di dwuma sɛ:** Nkitahodi a ɛwɔ "Show and tell" / "Architecture" ase, anaasɛ `docs/` ADR mfiase.
**Nneɛma a ɛho hia:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Nea enti a wɔyɛɛ ufolens.com sɛnea ɛteɛ no

Nkyerɛkyerɛmu a ɛfa anohyetoɔ mmiɛnsa a ɛhyɛɛ [ufolens.com](https://www.ufolens.com) no ho (nsɛm a wɔhwehwɛ mu, kasa ahodoɔ pii a wɔsan yɛɛ maa [PURSUE UAP kyerɛwtohɔ](https://www.war.gov/ufo)). Nsɛm a wɔka ne nsɛm a wɔpow nyinaa fata.

### 1. Pipeline no yɛ forward-only state machine — a wɔde atirimpɔw ayɛ

Gyinabea: `discovered → downloaded → ocr_done → translated → published`. Krataa bi kɔ anim nko ara, na ɛba bere a adwuma wɔ hɔ a ɛsɛ sɛ wɔyɛ no. Nsɛm a wɔatintim no, wɔnsan nyɛ ho adwuma bio gye sɛ delta detector hu sɛ source no asesa ankasa.

**Nea enti:** OCR + nkyerɛaseɛ yɛ adwuma a ɛyɛ den, na kyerɛwtohɔ no nyin bere kɔ so. Pipeline a ɛ"san yɛ biribiara bio sɛnea ɛbɛyɛ a ɛbɛyɛ den" no nni anohyetoɔ biara. Sɛ wɔma backward transitions yɛ nea ɛntumi nyɛ yiye a, ɛma ɛyɛ den sɛ wɔbɛbɔ ka kɛseɛ. Ka a wɔbɔ no ano yɛ state graph no su, na ɛnyɛ operator no ahwɛyie.

**Ka:** schema migrations ne reprocessing-on-purpose yɛ nea ɛyɛ den. Nsakrae a wotumi gye tom.

### 2. OCR ne nkyerɛaseɛ yɛ adwuma wɔ LLM a ɛwɔ mpɔtam hɔ so, na ɛnyɛ cloud API so

OCR: open-source engine, Tesseract CLI fallback. Nkyerɛase + NER: Gemma via Ollama, wɔ Apple Silicon laptop so.

**Nea enti:** sika a wɔbɔ wo wɔ krataa biara ho no nni hɔ; wotumi san yɛ (fixed model + prompts); na fetch gyinapɛn no, ɛsɛ sɛ ɛyɛ adwuma fi residential IP so (source no wɔ Akamai Bot Manager akyi — `curl` nya 403), enti laptop wɔ mu.

**Ka:** nkyerɛaseɛ no su nnu frontier model no deɛ so. Sɛ ɛyɛ reference corpus a Borɔfo a ɛhyɛ aseɛ no da hɔ bere nyinaa a, ɛno yɛ. Yɛnka sɛ nkyerɛaseɛ no yɛ nea ɛfata.

### 3. Afã abien no kyɛ interface baako pɛ: bundle a wɔatintim

Pipeline no nkyerɛw kɔ production database no mu tẽẽ da. Ɛde `{ SQL, asset manifest, cache-purge list }` ma. "Publishing" = fa saa bundle no di dwuma (push SQL kɔ edge SQL DB, sync assets kɔ object storage, yi cache keys a wɔahyɛ no agyirae no fi hɔ).

**Nea enti:** mpɔtam hɔ fã ne edge fã no tumi nyin wɔn anokwa mu; wotumi hwɛ bundle no mu; na "deploy data" no su yɛ pɛ bere nyinaa. Worker no yɛ TypeScript/Hono app ketewaa bi — CSP a ɛyɛ den (nni `unsafe-inline`; inline JSON-LD yɛ sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — na ɛnhia sɛ ɛhu sɛnea wɔyɛɛ data no.

**Ka:** D1 schema nsesaeɛ ka fael abien (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurance a ne bo yɛ fo.

### Nneɛma a ɛnsakra a wɔde ahyɛ su no mu

- Ɛne U.S. aban nni abusuabɔ biara; aban agyiraehyɛde biara nni hɔ.
- Wɔkora source redactions so, wɔnsan nkyerɛ bio da.
- Wɔde video no ma DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` wɔ beaeɛ no nyinaa — search-indexable, AI-scrape-opted-out.

Te ase: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
