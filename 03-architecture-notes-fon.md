# GitHub — Wema 3gɔ́ tɔn dó 3 mɛ · Nùɖe gbɛdido tɔn (ADR-style Goxɔ́)

**Nǔ zánzán:** Goxɔ́ ɖéé ɖò "Kúnnuɖenúmɔ & Nùɖe" / "Gbɛdido" glɔ́, alǒ `docs/` ADR bǐbɛ̌mɛ.
**Nùnywɛ́xó kléwúnkléwún lɛ́ɛ:** gbɛdido, ADR, forward-only state machine, LLM kɔ́mputá tɔn, Ollama, OCR, edge kɔ́mputá, CSP, nǔgbógbó sín nǔɖe, nǔkúnnúmɔ pipeline, nǔɖe así tɔn, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Nǔ e wu ufolens.com gbà ɖò lee è gbà ɛ gbɔn ɔ

Nǔ e è wlán dó nǔɖe atɔn e sɔ́ gbɛdido [ufolens.com](https://www.ufolens.com) (gbɛdido ayihun-jlɛ̌-kpɔn tɔn, gbèdagbe susu tɔn [PURSUE UAP gbɛdido](https://www.war.gov/ufo) tɔn) ɔ jí. Nùkúnnúmɔ / nǔsɔ́jlɔ́ lɛ́ɛ bǐ wɛ.

### 1. Pipeline ɔ nyí forward-only state machine — kpódó susu

Tɛn lɛ́ɛ: `discovered → downloaded → ocr_done → translated → published`. Wěma ɖokpó nɔ́ zɔn nukɔn kɛɖɛ, bɔ é nɔ́ nyí hwenu e azɔ̌ ɖéé ɖò fínɛ́ ɔ kɛɖɛ. Nǔ e è sɔ́ ɖ'ayǐ ɔ má nɔ́ zɔn gbeɖé ó, ényí dɔ́ delta detector ɖéé má mɔ dɔ́ asú ɔ huzu.

**Nǔ e wu ɔ:** OCR + nǔgbɔ́jelɔ́ wɛ nyí azɔ̌ e syɛn hugǎn lɛ́ɛ, bɔ gbɛdido ɔ nɔ́ yi nukɔn. Pipeline ɖéé e nɔ́ "wà nǔbǐ kpɔ́n bɔ é nǎ syɛn" ɔ kúnɖiɖo así e má ɖó dogbó ǎ ó. Hǔzúhúzú gúdo tɔn lɛ́ɛ sísɔ́ má sixú zɔn ó nɔ́ zɔn bɔ akwɛ́ e má ɖó dogbó ǎ ɔ má sixú nyí nùɖé ó. Así tɔn ɔ nyí nǔɖe e ɖò state graph ɔ mɛ, é má nyí azɔ̌wátɔ́ tɔn sín nǔɖe ó.

**Así:** schema migrations kpódó reprocessing-on-purpose lɛ́ɛ syɛn tawun. Nǔ e è sixú kɛnklɛn ɔ.

### 2. OCR kpódó nǔgbɔ́jelɔ́ nɔ́ zɔn LLM kɔ́mputá tɔn jí, é má nyí cloud API ɖéé jí ó

OCR: open-source mɛ́kini, Tesseract CLI fallback. Nǔgbɔ́jelɔ́ + NER: Gemma gbɔn Ollama, Apple Silicon laptop ɖéé jí.

**Nǔ e wu ɔ:** así ɖěbǔ má ɖò wěma ɖokpó jí ó; é sixú zɔn (model + prompts e ɖò fínɛ́ ɔ); bɔ fetch adà ɔ ko ɖó na zɔn sín IP nɔtɛn tɔn ɖéé mɛ (asú ɔ ɖò Akamai Bot Manager gúdo — `curl` nɔ́ mɔ 403), enɛ́ ɔ, laptop ɖéé ɖò azɔ̌ ɔ mɛ.

**Así:** nǔgbɔ́jelɔ́ jlɛ̌jlɛ̌ tɔn ɖò model frontier ɖéé glɔ́. Gbɛdido ɖéé mɛ e English gbè asú tɔn ɔ ɖò fínɛ́ gbè ɖokpó ɔ, é sɔgbe. Mǐ má ɖɔ dɔ́ nǔgbɔ́jelɔ́ lɛ́ɛ nyí nǔgbó ó.

### 3. Adà we lɛ́ɛ nɔ́ má nǔɖe ɖokpó pɛ́pɛ́pɛ́: bundle ɖéé è sɔ́ ɖ'ayǐ

Pipeline ɔ má nɔ́ wlán nǔ dó azɔ̌ database tɔn ɔ mɛ gbeɖé ó. É nɔ́ sɔ́ `{ SQL, asset manifest, cache-purge list }` sínyɛ́nyɛ́. "Nǔsɔ́ɖ'ayǐ" = sɔ́ bundle enɛ́ ɔ dó nukɔn (sɔ́ SQL dó edge SQL DB mɛ, sɔ́ assets dó nǔbǐ agbasa mɛ, sɔ́ cache keys e è ylɔ́ ɔ sínsɛ́n).

**Nǔ e wu ɔ:** kɔ́mputá adà ɔ kpódó edge adà ɔ kpódó sixú yi nukɔn kpódó yeɖesunɔ; bundle ɔ sixú kpɔ́n; bɔ "sɔ́ nùkúnnúmɔ dó ayǐ" ɔ ɖò alɔkpa ɖokpó ɔ mɛ gbè bǐ gbè. Worker ɔ nyí TypeScript/Hono app kpɛví ɖéé — CSP syɛnsyɛn (é má nyí `unsafe-inline` ó; inline JSON-LD sha256-pinned), `Accept-Language` + ayikúngban→gbèdàgbe susu, dɔ̌nkpɛkɛ́n KV wémata tɔn gbèzán gban (30), cron xɔ́gbèzán tɔn — bɔ é má byɔ́ na tuùn lee è wà nùkúnnúmɔ ɔ gbɔn gbeɖé ó.

**Así:** D1 schema hǔzúhúzú ɖéé nɔ́ jɛ wěma we wu (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Nǔɖe e así tɔn má syɛn ǎ ɔ.

### Nǔ e má sixú huzu ǎ lɛ́ɛ e ɖò nǔɖe mɛ ɔ

- Kún kɔn U.S. axɔ́súɖuto ɔ ó; nǔɖe sín nǔɖe sín nùɖé má ɖò fínɛ́ ó.
- Asú redactions lɛ́ɛ ɖò fínɛ́, è má nɔ́ lɔ́ ye gúdo gbeɖé ó.
- Video e è sɔ́ dó DVIDS / AARO sí ɔ.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` tɛn ɔ bǐ jí — ayihun-jlɛ̌-kpɔntɔ́ lɛ́ɛ sixú mɔ ɛ, AI-scrape-opted-out.

Gbɛtɛn: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
