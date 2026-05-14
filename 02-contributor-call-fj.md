# GitHub — iVolau 2 vei 3 · Veisureti vei ira na dauveivuke / "good first issues"

**Na kenai vakayagataki:** me vaka e dua na Veivosaki e vakaduri ("Veivuke kei na kena digitaki na veika bibi ni cakacaka taumada") se dua na kena ivakamacala ni CONTRIBUTING.md.
**Vosa bibi:** open source, veivuke, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Na kena soqoni:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Veivuke ki na ufolens.com

[ufolens.com](https://www.ufolens.com) e na veisautaka na ivolatukutuku ni Vavakoso ni Valu e Amerika na [PURSUE UAP archive](https://www.war.gov/ufo) me dua na vanua e rawa ni vakasaqaqarataki, levu na kena vosa, ka tiko talega kina na [public API](https://www.ufolens.com/api/v1). E rua na kena iwasewase — na local Python ingest pipeline (`pipeline/`) kei na TypeScript/Hono edge app (`worker/`) — e na sota ena dua ga na kena interface: na SQL e tabaki tu kei na bundle ni iyaya.

E sega ni gadrevi e dua na cloud credentials mo ni veivuke kina. Na core modules ni pipeline e stdlib-only ka na cicivaki na Worker tests ena loma ni memori.

### Vakarautaki

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Na vanua e yaga sara na veivuke

**i18n / localization** — `worker/src/i18n/ui-strings.json` na ivurevure ni ui strings. Na kena vakaraici mai vei ira era vosa vakaidewadewa ena veivosa eso e yaga sara: me rawa ni kunea na vosa e sega ni yaga na kena vakadewa vakamisini, me vakavinakataki na veika e tarai ena RTL/layout, me vakavinakataki na veika e tarai ena language-negotiation.

**Na ituvaki ni OCR** — me vakavinakataki na kena vakarautaki taumada na ivola makawa e volai vakamisini ni bera na OCR; na kena vakatovolei na open-source engine ni veisemati kei na Tesseract fallback ena ivola ni vakatovolei.

**Na accessibility** — me vakaraici na rendered pages (`worker/src/render/`) me veiraurau kei na WCAG; na CSP e strict (sega ni `unsafe-inline`), me rawa ni cakacaka na veika e rawa ni vakavinakataki kina.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Na kaukauwa ni pipeline** — me levu na kena graceful-degradation paths, me vakavinakataki na kena tukuni na kena cicivaki, na delta-detection edge cases (`pipeline/lib/delta.py`).

**iVola** — `docs/20260511/` (繁體中文; `00-*` na index). E na ciqomi na kena vakadewataki na ivola ni design ki na vosa vaka-Peritania.

### iVakatagedegede ni ivakarau

- Na veika kece e dodonu me veisemati — e dodonu me rawa ni veitosoyaki na cakacaka ena veimisini kece. Sega ni na vakayagataki na hardcoded absolute paths.
- E kakua ni biuta e dua na pip dependency ki na pipeline *core* module. Na optional stages e rawa ni vakayagataka na optional packages, ka na dodonu me na rawarawa na kena vakayagataki ni sega ni tiko kina.
- E kakua ni vakalolomataka na forward-only state machine — oqori na kena iwiliwili ni veika e na vakayagataki.
- E kakua ni biuta e dua na ivakatakilakila ni matanitu e Amerika, ka kakua ni biuta e dua na veika e na veisautaka na ivola dina.
- Na veisau ni D1 schema e na tarai **rua** na ivola: `pipeline/lib/manifest_schema.sql` kei na `db/schema.sql`.
- Vakatovolei ena code vou. Conventional-commit messages.

Wilika na `CLAUDE.md` kei na `docs/20260511/00-*` taumada, oti na kena dolavi e dua na issue mo veivosakitaka na veisau ni cakacaka ni bera na PR.

