# GitHub — iVolau 1 vei 3 · Tukutuku ni Vakalesu · iTukutuku ni Vakadewa

**Na kenai vakayagataki:** na GitHub Release, na Veivosaki e vakaduri, se na ulutaga ni repo README.
**Vosa bibi:** UAP, UFO, PURSUE archive, ivola e tabaki tu me baleti ira na lewenivanua, data digitaki, vakasaqaqara vakaivola taucoko, OCR, vakadewa vakamisini, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Na kena soqoni:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — dua na vanua e levu na kena vosa, rawa ni vakasaqaqarataki ena vakasarana o PURSUE UAP

**Bula tiko:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **iTuvatuva ni kena ivurevure:** https://www.war.gov/ufo

`ufolens.com` e tabaka tale na ivolatukutuku ni Vavakoso ni Valu e Amerika na **PURSUE** mai na ivola e sa tabaki tu me baleta na UAP/UFO me vaka e dua na vanua ni kilaka: vakasaqaqara vakaivola taucoko, vakadewa vakamisini ena veitikina taucoko, raica na mape kei na iTuvatuva ni Gauna, kei na public JSON API. Na ivola dina e sa cakacaka ni matanitu e Amerika ka sa tu ena vanua ni lewenivanua ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Na cakacaka qo **e sega ni veisemati kei na matanitu e Amerika**, e sega ni vakayagataka na kena ivakatakilakila, ka sega ni veisautaka na veika e tabaki koto kina.

### iVakatakilakila

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

- **Sega ni levu na iwiliwili ni cloud-AI ena veivola taucoko.** Na OCR kei na vakadewa e cicivaki ena loma ni komupi; na ivakatakilakila ni forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) e na vakadeitaka ni sega ni na cakacakataki tale e dua na ivola, vakavo ga ke sa veisau.
- **Na iwasewase bibi ni pipeline e sega ni vakayagataka na third-party dependencies** — na parsing / manifest / delta modules e na cicivaki ka na vakatovolei ena dua na Python e savasava ka sega ni pip-installed; na OCR/translation stages e na vakararawataki ni sega ni tiko na optional packages.
- **Na vanua ni Edge** e na vakayagataka na strict security headers + CSP (sega ni `unsafe-inline`; na inline JSON-LD e sha256-pinned), na veivosaki ni vosa ena `Accept-Language` + na veisemati ni vanua, na 30-day KV page cache, kei na daily housekeeping cron.
- **Na veisau e lailai:** e dua na delta detector e na raica na veisau ena source index ka na vakauta ga na veisau ki na pipeline.

### Vei ira na dauvakacakacaka

Na public API ena https://www.ufolens.com/api/v1 e na vakauta na ivola kei na metadata me vaka na JSON. Na kena vakayagataki vakaca e na toqai; kerea e dua na idusidusi ni vanua vei ira na dauvakasaqaqara/dauvakacakacaka. Raica na iwasewase ni API ena vanua ni veisaqata kei na veika e rawa ni cakava.

### iTuvaki

Sa oti na code; na vanua e sa tu ena https://www.ufolens.com. Na ivakatakilakila ni vuli e na vakasokumuni ena kena cicivaki na offline pipeline ka tabaka na bundle ki liu (`cli_publish run --remote`). Na ivola taucoko ni design e na tu ena `docs/20260511/`.

### Laiseni / iyalayala

- Na ivurevure ni ivola: cakacaka ni matanitu e Amerika, ka sa tu ena vanua ni lewenivanua ena loma ni Amerika.
- Na code ni vanua oqo: raica na `LICENSE`.
- Na vanua e na vakauta na `Tdm-Reservation: 1` kei na `X-Robots-Tag: noai, noimageai` — rawa ni vakasaqaqarataki ena search engines, ka sega ni kau mai na AI training/scraping.
- Na vidio e na dodonu me na qaqalo mai na DVIDS / AARO ka sega ni na qai vakayagataki ena cakacaka oqo.

Na leqa kei na PRs e na ciqomi. Yalovinaka ni wilika na `CLAUDE.md` kei na `docs/20260511/00-*` ni bera ni o dolava na veisau ni cakacaka.

