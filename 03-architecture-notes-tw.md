# GitHub — Nsɛm a ɛtɔ so 3 wɔ 3 mu · Nsɛm a ɛfa adansi ho (ADR-style Discussion)

**Fa di dwuma sɛ:** Nsɛm a Wɔka Kyerɛ wɔ "Kyerɛ na ka" / "Adansi" ase, anaa `docs/` ADR aba.
**Atitiriw a wɔde di dwuma:** adansi, ADR, forward-only state machine, mpɔtam hɔ LLM, Ollama, OCR, edge computing, CSP, ahobammɔ ho nsɛmti, data pipeline, sika a wɔsɛe no ho mfiridwuma, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Adɛn nti na wɔasisi ufolens.com sɛnea ɛte no

Nsɛm a ɛfa gyinaesi abiɛsa a ɛkyerɛɛ [ufolens.com](https://www.ufolens.com) (wɛbsaet a wotumi hwehwɛ mu, kasa pii a wɔsan sii maa [PURSUE UAP archive](https://www.war.gov/ufo)) no ho. Nsɛm a wɔka / ahyɛde a wɔde ma no ho kwan.

### 1. Pipeline no yɛ forward-only state machine — a wɔahyɛ da ayɛ

Tebea ahorow: `discovered → downloaded → ocr_done → translated → published`. Krataa bi kɔ anim nkutoo, na ɛba bere a adwuma wɔ hɔ a ɛsɛ sɛ wɔyɛ no nkutoo. Wɔnsan nyɛ nsɛm a wɔatintim no da gye sɛ delta detector hu sɛ fibea no asesa ampa.

**Adɛn nti:** OCR + nkyerɛase ne nnwuma a ɛsɛe sika kɛse, na bere kɔ so no, nneɛma a wɔkora so no yɛ kɛse. Pipeline a "ɛsan yɛ biribiara ma ɛyɛ ahobammɔ" no sika a wɔsɛe no nni ano. Sɛ wɔma akyi nsakrae yɛ nea entumi nyɛ yiye a, ɛma sika a wɔbɔ no boro so no yɛ nea entumi nyɛ yiye. Sika a wɔsɛe no ano no yɛ tebea no mfonini no su, na ɛnyɛ nea ɔyɛ adwuma no ani da hɔ.

**Sika a wɔbɔ:** adansi mu nsakrae ne adwuma a wɔsan yɛ no ahyɛ da ayɛ no yɛ nea ɛyɛ nwonwa. Nsesae a wotumi gye tom.

### 2. OCR ne nkyerɛase di dwuma wɔ LLM a ɛwɔ mpɔtam hɔ so, na ɛnyɛ cloud API so

OCR: open-source engine, Tesseract CLI fallback. Translation + NER: Gemma via Ollama, on an Apple Silicon laptop.

**Adɛn nti:** krataa biara nni sika a wɔsɛe no; wotumi san yɛ (model a wɔahyɛ da ayɛ + prompts); na fetch step no nyinaa wɔ hɔ a ɛsɛ sɛ ɛkɔ so fi residential IP (fibea no wɔ Akamai Bot Manager akyi — `curl` nya 403), enti laptop bi wɔ loop no mu.

**Sika a wɔbɔ:** nkyerɛase su no wɔ ɔhye so model ase. Sɛ yɛde kyerɛwtohɔ bi a Engiresi kasa a edi kan no daa yɛ klik biako pɛ a, ɛno yɛ yiye. Yɛnkyerɛ sɛ nkyerɛase ahorow no yɛ nea ɛwɔ tumi.

### 3. Nnipadua no fã abien no nyinaa kyɛ interface biako pɛ: bundle a wɔatintim

Pipeline no nkyerɛw kɔ production database no so tẽẽ da. Ɛde `{ SQL, asset manifest, cache-purge list }` ma. "Publishing" = de saa bundle no di dwuma kɔ anim (push SQL kɔ edge SQL DB so, sync assets kɔ object storage so, yi cache keys a wɔde din ahyɛ mu no fi hɔ).

**Adɛn nti:** mpɔtam hɔ fã ne ano fã no betumi anyin wɔn ho wɔn ho; wotumi hwɛ bundle no mu; na "deploy data" no te sɛ nea ɛte bere biara. Worker no yɛ TypeScript/Hono app ketewaa bi — CSP a emu yɛ den (no `unsafe-inline`; inline JSON-LD yɛ sha256-pinned), `Accept-Language` + ɔman→kasa mu apereperedi, nnafua 30 KV kratafa cache, da biara da ofie adwuma cron — na enhia da sɛ ɛbɛhu sɛnea wɔyɛɛ data no.

**Sika a wɔbɔ:** D1 adansi mu nsakrae ka fael abien (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurance a ne bo nyɛ den.

### Nneɛma a wontumi nni ho adwinni a wɔde ahyɛ suban mu

- Ɛne U.S. aban nni abusuabɔ; aban agyiraehyɛde biara nni hɔ.
- Wɔkora nsɛm a wɔde akyerɛw no so, na wɔnsan nkyerɛw bio da.
- Video a wɔde ama DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` wɛbsaet no nyinaa — wotumi hwehwɛ mu, wɔayi wɔn afi AI a wɔde hwehwɛ nneɛma mu.

Ɛwɔ wɛbsaet so tẽẽ: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
