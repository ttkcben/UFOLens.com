# GitHub — Wema 1tɔn dó 3 mɛ · Nùɖeɖe sínyɛ́nyɛ́ / README blɔ́kù tɔn

**Nǔ zánzán:** GitHub Release gbɛhan, goxɔ ɖéé ɖɔ è slɔ́, alǒ bǐbɛ̌mɛ repo README tɔn.
**Nùnywɛ́xó kléwúnkléwún lɛ́ɛ:** UAP, UFO, PURSUE wěmaxɔ́, wěma ɖěé è ɖè sínyɛ́nyɛ́ nú lɛ́ɛ, nùkúnnúmɔ jɛ̌gbɔ́jí tɔn, bǐbɛ̌má tɛ́kítì tɔn, OCR, nǔgbɔ́jelɔ́ mɛ́kini tɔn, LLM kɔ́mputá tɔn, Ollama, kɔ́mputá sísɛ́ tɔn, API gbɔ́ngbɔ́n tɔn, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — gbɛdohɛn gbèdagbe susu tɔn, ye ɖò ayihun-jlɛ̌-kpɔn tɔn na PURSUE UAP gbɛdido

**Gbɛtɛn:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Gbɛdido asú tɔn:** https://www.war.gov/ufo

`ufolens.com` nɔ́ gbɛ́ nǔgbó U.S. War Department tɔn **PURSUE** wěmaxɔ́ UAP / UFO wěma ɖěé è ɖè sínyɛ́nyɛ́ nú lɛ́ɛ sín nǔgbó nǔnywɛ́ gbɛdohɛn ɖéé: bǐbɛ̌má tɛ́kítì tɔn, nǔgbɔ́jelɔ́ mɛ́kini tɔn gbɔn gbɛdido bǐ mɛ, ayikúngban + hwenu-gbɛdohɛn ayihun-jlɛ̌-kpɔn, kpódó API JSON gbɔ́ngbɔ́n tɔn ɖéé. Wěma asú lɛ́ɛ nyí azɔ̌ U.S. axɔ́súɖuto tɔn lɛ́ɛ, bó ɖò U.S. mɛ ɔ, gbɔ́ngbɔ́n mɛ e ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Azɔ̌ elɔ́ **kún kɔn U.S. axɔ́súɖuto ɔ ó**, é kún zán nùɖé sín nùɖé ó, é ká nɔ́ lɔ́ nùɖé sín nùɖé gúdo gbeɖé ó.

### Nǔgbógbó tɔn

```
Mɛ́kini kɔ́mputá tɔn (Apple Silicon, IP nɔtɛn tɔn)    Azozɛ́ edge tɔn
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   wémata
  OCR: open-source mɛ́kini (Tesseract CLI fallback)    /api/v1/...   API gbɔ́ngbɔ́n tɔn
  translate / NER: LLM kɔ́mputá tɔn (Gemma gbɔn Ollama)   /admin        azɔ̌wátɔ́ tɔn
  state: SQLite manifest                             sɔ́ mɛ: edge SQL DB, nǔbǐ
        │                                              tɔn (source PDFs), KV cache
        └── nɔ́ sɔ́ nǔɖé sínyɛ́nyɛ́: SQL + nǔɖe manifest + cache-purge list ──┘
```

- **Súkpɔ́ súkpɔ́ nǔɖe alɔkpa ɖokpó e ɖò alɔkpa mɛ ɔ, é kúnɖiɖo nǔɖe ó.** OCR kpódó nǔgbɔ́jelɔ́ ɔ kɛɖɛ wɛ nɔ́ zɔn; mɛ́kini forward-only state (`discovered → downloaded → ocr_done → translated → published`) nɔ́ zɔn bɔ nǔɖe má sɔ́ nɔ́ zɔn gbeɖé ó, ényí dɔ́ é huzu.
- **Pipeline core kúnɖiɖo nǔɖe sín alɔkpa atɔngɔ́ ɔ sín nǔɖe ɖě ó** — parsing / manifest / delta modules nɔ́ zɔn, bó nɔ́ kpɔ́n Python mímɛ́ ɖéé jí bɔ nǔɖe sín nǔɖe má zɔn ó; OCR/translation adà lɛ́ɛ nɔ́ gbɔjɛ́ kpódó nǔɖe lɛ́ɛ ɖò fínɛ́ hwenu e nǔɖe lɛ́ɛ má ɖò fínɛ́ ǎ ɔ.
- **Edge tɛn ɔ** nɔ́ zán nǔɖe sín nǔɖe kpódó CSP kpódó (é má nyí `unsafe-inline` ó; inline JSON-LD sha256-pinned), gbèdàgbe susu tɔn gbɔn `Accept-Language` + ayikúngban mapping, dɔ̌nkpɛkɛ́n KV wémata tɔn gbèzán gban (30), kpódó cron xɔ́gbèzán tɔn.
- **Nǔgbó yɔ́yɔ́ lɛ́ɛ:** delta detector ɖéé nɔ́ ɖè vovo sín asú index ɔ mɛ, bó nɔ́ sɔ́ hǔzúhúzú lɛ́ɛ kɛɖɛ dó pipeline ɔ mɛ.

### Nǔgbógbótɔ́ lɛ́ɛ tɔn

API gbɔ́ngbɔ́n tɔn ɖò https://www.ufolens.com/api/v1 nɔ́ sɔ́ wěma lɛ́ɛ kpódó metadata lɛ́ɛ dó JSON mɛ. Nǔbǐbɛ̌má gbɔ́ngbɔ́n tɔn lɛ́ɛ ɖò nǔɖe mɛ; byɔ́ kléwún ɖéé nú ayihun-jlɛ̌-kpɔntɔ́/nǔgbógbótɔ́ lɛ́ɛ. Kpɔ́n API adà e ɖò tɛn ɔ jí ɔ nú nǔbǐbɛ̌má kpódó dogbó lɛ́ɛ.

### Tɛn tɔn

Kɔ́dù ɔ bǐ wɛ; tɛn ɔ ɖò https://www.ufolens.com jí. Azɔ̌ database tɔn ɔ nɔ́ zɔn gbɔn offline pipeline ɔ zízán kpódó bundle ɔ sísɔ́ dó nukɔn (`cli_publish run --remote`). Nǔgbógbó wěma bǐ ɖò `docs/20260511/` mɛ.

### Gbɛdido / dogbó lɛ́ɛ

- Wěma asú lɛ́ɛ: U.S. axɔ́súɖuto tɔn azɔ̌ lɛ́ɛ, gbɔ́ngbɔ́n mɛ ɖò U.S. mɛ.
- Gbɛdohɛn elɔ́ tɔn kɔ́dù ɔ: kpɔ́n `LICENSE`.
- Tɛn ɔ nɔ́ sɛ́dó `Tdm-Reservation: 1` kpódó `X-Robots-Tag: noai, noimageai` — ayihun-jlɛ̌-kpɔntɔ́ lɛ́ɛ sixú mɔ ɛ, é ká gbɛ́ AI nǔkplɔ́nkplɔ́n/nǔbǐbɛ̌má.
- Video wěma lɛ́ɛ ɖò DVIDS / AARO sí, bɔ azɔ̌ elɔ́ má byɔ́ ɛ ó.

Nǔgbó kpódó PRs lɛ́ɛ bǐ wɛ. Kɛnklɛn xà `CLAUDE.md` kpódó `docs/20260511/00-*` kóɖó hun hǔzúhúzú ɖěbǔ.

