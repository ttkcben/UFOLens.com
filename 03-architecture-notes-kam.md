# GitHub — Mwandĩko wa 3 wa 3 · Marũa ma ũcũthĩrĩria (Kwĩrangĩria kĩa ADR-style)

**Kũhũthĩra ta:** Kwĩrangĩria thĩinĩ wa "Kũrora na kũruta" / "Ũcũthĩrĩria", kana `docs/` ADR seed.
**Ciugo cia bata:** ũcũthĩrĩria, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Nĩkĩ ufolens.com ĩcũthĩrĩriwĩte na njĩra ĩrĩa ĩrĩ

Marũa ma njũngwa itatũ ciarũgamĩrĩirie [ufolens.com](https://www.ufolens.com) (kũcoka kũcũrania kĩa ndimi nyingĩ kĩa [PURSUE UAP archive](https://www.war.gov/ufo)). Kwĩrangĩria / kũcoka nĩ ngũĩrĩra.

### 1. Mũthingi wa pipeline nĩ mũcinyi wa itata rĩa gũthiĩ mbele — na kũcũrania

Itata: `discovered → downloaded → ocr_done → translated → published`. Marũa nĩ ngũĩrĩra o mbele, na o rĩrĩa kũrĩ wĩra wa kũruta. Content ĩrutĩtwo ndĩcoki kũcũrania o na iharĩka o na delta detector ĩrora kĩhumo kĩa kũgarũrũka.

**Nĩkĩ:** OCR + gũcũrania nĩ wĩra ũrĩa ũrĩ wa gĩthĩtĩra, na rũma rwa gĩthita nĩ ngũĩrĩra kũruta. Pipeline ĩrĩa "ĩcoka kũruta kĩndũ kĩmwe kĩmwe nĩ ũndũ wa kũgĩa na ũtharĩki" ĩrĩ na gĩthĩtĩra kĩa kĩbata. Kũruta kũcoka gũcũrania gũtirĩ na njũngwa ĩmwe nĩ ngũĩrĩra kũruta kĩbata kĩa kĩbata. Gĩthĩtĩra kĩa gĩthĩtĩra nĩ gĩa state graph, ti gĩa operator vigilance.

**Gĩthĩtĩra:** schema migrations na reprocessing-on-purpose nĩ njũngwa njũgũmĩre. Tradeoff ĩrĩ ngũĩrĩra.

### 2. OCR na gũcũrania nĩ ngũĩrĩra kũrĩa local LLM, ti cloud API

OCR: open-source engine, Tesseract CLI fallback. Gũcũrania + NER: Gemma na Ollama, kũrĩa Apple Silicon laptop.

**Nĩkĩ:** tũtirĩ na gĩthĩtĩra kĩa kĩbata kĩa marũa othe; kũruta (fixed model + prompts); na fetch step nĩ ngũĩrĩra kũruta kũrĩa residential IP (kĩhumo nĩ Akamai Bot Manager — `curl` ĩmũkĩra 403), nĩ ũndũ wa ũrĩa laptop ĩrĩ thĩinĩ wa loop o na iharĩka.

**Gĩthĩtĩra:** ũthaka wa gũcũrania nĩ ngũĩrĩra kũrĩa frontier model. Nĩ ũndũ wa reference corpus kũrĩa Gĩthũngũ kĩa mbere kĩrĩ kũthũũranĩka, ũrĩa nĩ mũthĩnĩ. Tũtirĩ na kĩhumo kĩa gũcũrania.

### 3. Icunjĩ igĩrĩ nĩ ngũĩrĩra interface ĩmwe: bundle ĩrutĩtwo

Pipeline ndĩrĩ ngũĩrĩra kũandĩka kũrĩa production database gũthathaũka. Ĩrutithagia `{ SQL, asset manifest, cache-purge list }`. "Kũrutithia" = kũhũthĩra bundle ĩrĩa mbele (kũrutithia SQL kũrĩa edge SQL DB, sync assets kũrĩa object storage, purge the named cache keys).

**Nĩkĩ:** local side na edge side nĩ ngũĩrĩra kũruta mbele kũhũthĩra; bundle nĩ ngũĩrĩra kũrora; na "deploy data" nĩ mũgarũrũko mũmwe o na iharĩka. Worker nĩ TypeScript/Hono app njerũ — strict CSP (gũtirĩ `unsafe-inline`; inline JSON-LD nĩ sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — na ndĩrĩ ngũĩrĩra kũmenya ũrĩa data ĩrutĩtwo.

**Gĩthĩtĩra:** D1 schema change nĩ ngũĩrĩra marũa igĩrĩ (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Insurance ĩrĩ ngũĩrĩra.

### Njũngwa iria itarĩ ngũĩrĩra kũgarũrũka

- Ndĩrĩ na kĩhumo na thirikari ya U.S.; gũtirĩ rũũri rwa thirikari.
- Maũndũ marutĩtwo nĩ ngũĩrĩra kũhũthĩra, ndĩcoki kũcoka gũthathaũra.
- Video ĩrĩa ĩrĩ DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` thĩinĩ wa site — kũthũũranĩka na search, AI-scrape-opted-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
