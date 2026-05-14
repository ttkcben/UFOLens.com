# GitHub — Mwandĩko wa 1 wa 3 · Kũruta / Kwĩmenyesha gĩa README

**Kũhũthĩra ta:** mwĩrĩ wa Kũruta gĩa GitHub, Kũrũmia Kwĩrangĩria, kana igũrũ gĩa README gĩa repo.
**Ciugo cia bata:** UAP, UFO, rũma rwa PURSUE, marũa marutĩtwo gĩthita, data ĩtarĩ ya hitho, kũthũũra maũndũ ma kwandĩka, OCR, gũcũrania na macini, LLM ya mũciĩ, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — gĩcunjĩ kĩa ndimi nyingĩ, kĩrĩ kũthũũranĩka kĩa rũma rwa PURSUE UAP

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Source archive:** https://www.war.gov/ufo

`ufolens.com` ĩcoka kũrutithia rũma rwa U.S. War Department's **PURSUE** rwa marũa marutĩtwo gĩthita ma UAP / UFO ta kĩcunjĩ kĩa ũmenyi: kũthũũra maũndũ ma kwandĩka, gũcũrania na macini thĩinĩ wa marũa mothe, kũrora mũtĩ wa ndarama + mĩmera ya hĩndĩ, na JSON API ya bũrũri. Marũa ma kĩhumo nĩ wĩra wa thirikari ya U.S. na thĩinĩ wa U.S. nĩ ma mũthenya wa bũrũri ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Wĩra ũyũ **ndũrĩ na kĩhumo na thirikari ya U.S.**, ndũhũthagĩra rũũri rwa thirikari, na ndũcoki gũthathaũra maũndũ marutĩtwo.

### Ũcũthĩrĩria

```
Local machine (Apple Silicon, residential IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- Tũtirĩ na gĩthĩtĩra kĩa cloud-AI kĩa marũa othe. OCR na gũcũrania nĩ ngũĩrĩra kũrĩa kũrĩ mũciĩ; mũcinyi wa itata rĩa gũthiĩ mbele (`discovered → downloaded → ocr_done → translated → published`) ũrũgamĩrĩire atĩ gũtirĩ marũa macokagia kũcũrania o na iharĩka o na itacokagia kũgarũrũka.
- Mũthingi wa pipeline ndũrĩ na ũcũrania wa andũ angĩ — parsing / manifest / delta modules nĩ ngũĩrĩra na kũtata thĩinĩ wa Python ĩtharĩte ĩtarĩ na kĩndũ kĩtũmĩtwo na pip; OCR/translation stages nĩ ngũĩrĩra o na iharĩka o na kũgarũrũka nĩ ũndũ wa kũtiga packages ĩrĩ cia bata.
- Edge site nĩ ngũĩrĩra security headers + CSP (gũtirĩ `unsafe-inline`; inline JSON-LD sha256-pinned), kũcũrania rũthiomi na `Accept-Language` + kũrora bũrũri, 30-day KV page cache, na cron ya kũrũgamĩrĩra mũthenya o mũthenya.
- Kũgarũrũka kũthĩĩte mbele: delta detector nĩ ngũĩrĩra rũma rwa kĩhumo na kũrutithia o kũgarũrũka kũrĩa kũrĩ bata thĩinĩ wa pipeline.

### Kwa andũ a kũcũrania

Public API kũrĩa https://www.ufolens.com/api/v1 nĩ ngũĩrĩra marũa na metadata ta JSON. Kũingĩra kwa andũ arĩa matarĩ andũ a gũcũrania nĩ kũgĩrĩthĩtwo; kũmũkĩra gĩtĩma kĩa researcher/developer tiers. Rora kĩcunjĩ kĩa API kũrĩa site nĩ ũndũ wa endpoints na limits.

### Ũrirũ

Code ĩthirĩtwo; site ĩtũmĩtwo kũrĩa https://www.ufolens.com. Production database nĩ ngũĩrĩra kũhũthĩra offline pipeline na kũrutithia bundle mbele (`cli_publish run --remote`). Marũa ma kũcũrania mothe marĩ kũrĩa `docs/20260511/`.

### License / mĩhaka

- Marũa ma kĩhumo: wĩra wa thirikari ya U.S., mũthenya wa bũrũri thĩinĩ wa U.S.
- Code ya platform ĩyĩ: rora `LICENSE`.
- Site ĩtũmaga `Tdm-Reservation: 1` na `X-Robots-Tag: noai, noimageai` — ĩrĩ kũthũũranĩka na search engines, ĩrũgamĩrĩtwo gũtũmĩra AI training/scraping.
- Video footage nĩ ngũĩrĩra DVIDS / AARO na ndĩkũhũthĩra wĩra ũyũ.

Issues na PRs nĩ ngũĩrĩra. Thoma `CLAUDE.md` na `docs/20260511/00-*` mbere ya kũgũra kũgarũrũka gĩa mũthingi.

