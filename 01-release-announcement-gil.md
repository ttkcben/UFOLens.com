# GitHub — Te Kaongora 1 n te 3 · Te kabwarabwara n te kabobonga / Te bwaiko n te kaongo n te README

**Kabongana bwa:** te kaongora n te kabobonga n te GitHub, te kamanaua n te kaukukurei, ke te bitina n te repo README.
**Taeka n rairaki:** UAP, UFO, te waaki n te PURSUE, nakoan rikirake ake a koreta, taeka n reirei ae ukeuke, ukoukurei ae koroboki n aki toki, OCR, rairaki n te miti, LLM n te aono, Ollama, kabongana n te komputa n te keang, API ae bwareta, Hono, TypeScript, Python
**Teuati ni kanoa:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — te aaki ae taetae n aki tokanako, ae kakukurei iaon te waaki n te PURSUE UAP

**Maiu:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Waaki n roko:** https://www.war.gov/ufo

`ufolens.com` e kabwarabwara riki n te boki te waaki n te U.S. War Department ae **PURSUE** nakoan rikirake n te UAP / UFO ake a koreta bwa te aaki n te atatai: ukoukurei ae koroboki n aki toki, rairaki n te miti iaon te koroboki, mabi + ukoukurei n te tai, ao te API ae bwareta n te JSON. Nakoan rikirake ake a roko bon te mwakuri n te tautua n te U.S. ao i nanon te U.S. bon te bwareta n aki toki ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Te waaki aio **e aki ma te tautua n te U.S.**, e aki kabongana te aomata n te tautua, ao e aki rairaki n te boki nakoan rikirake ake a koreta.

### Te karaki

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

- **Te aki bwa te kabanea n te kabongana n te cloud-AI nakoan rikirake.** OCR ao rairaki e eke i nanon te aono; te aaki n te tai ae kaai bwa `discovered → downloaded → ocr_done → translated → published` e kamanena bwa e aki reke te boki nakoan rikirake n te kainakinakin bwa e aki rairaki.
- **Te pipeline core e aki bon te kainakinakin n te tangira n te aomata** — kaaitaraan te boki nakoan rikirake / te kabwarabwara / te delta modules e eke ao e kabwarabwara iaon te Python ae kaiririki n aki te kainakinakin n te pip-installed; te OCR/translation stages e aki bon te kainakinakin n te kaikawa n te aomata n te kainakinakin n te aomata n te aomata.
- **Te Edge site** e kabongana te kaikawa n te taetae n te CSP (aki `unsafe-inline`; te JSON-LD ae kaaitaraan e sha256-pinned), te taetae n te `Accept-Language` + te kaikawa n te aono, te KV page cache ae 30 n te ranna, ao te cron ae tabe n te ranna n te aia.
- **Te kaibakaki n te kaikawa:** te delta detector e rairaki n te roko n te waaki n te aomata ao e aki kaikawa n te kaikawa n te aomata.

### Ni kabwarabwara

Te API ae bwareta i https://www.ufolens.com/api/v1 e rairaki n te boki nakoan rikirake ao te metadata bwa te JSON. Te kabongana n te aomata bon te kaikawa n te rate; kabwarabwara te key ni kabwarabwara n te researcher/developer tiers. Kaaitaraan te API i nanon te site ni kabwarabwara n te kabwarabwara n te kabwarabwara.

### Te tai

Code e oti n aki toki; te site e kaikawa i https://www.ufolens.com. Te database n te kabobonga e kaikawa n te kabwarabwara n te offline pipeline ao e kaikawa n te kabobonga n te bundle bwa te `cli_publish run --remote`. Nakoan rikirake n te design docs e kaikawa i `docs/20260511/`.

### Te rairaki / te kaikawa

- Nakoan rikirake: te mwakuri n te tautua n te U.S., te bwareta n aki toki i nanon te U.S.
- Te code n te aaki aio: kaaitaraan `LICENSE`.
- Te site e rairaki `Tdm-Reservation: 1` ao `X-Robots-Tag: noai, noimageai` — kaibakaki n te search engines, e aki kaibakaki n te AI training/scraping.
- Te video footage e kabwarabwara n te DVIDS / AARO ao e aki kabwarabwara n te waaki aio.

Issues ao PRs a kaikawa. Kaaitaraan `CLAUDE.md` ao `docs/20260511/00-*` bwa te kabwarabwara n te kabwarabwara.

