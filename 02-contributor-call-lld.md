# GitHub — Post 2 de 3 · Cri de contribuimënter / "bon prum issues"

**Dovrar coche:** na discussion fisseda ("Contribuzions & bon prum issues") o na introduzion a CONTRIBUTING.md.
**Paroles clau:** open source, contribuì, bon prum issue, i18n, localisazion, OCR, Python, TypeScript, Vitest, pytest, azessibilità, UAP, open data
**Coleganc:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Co contribuì a ufolens.com

[ufolens.com](https://www.ufolens.com) trasforma l [archif PURSUE UAP](https://www.war.gov/ufo) del Departimënt de viera di Stac Unic en na piattaforma consultabla y plurilingue cun na [API publica](https://www.ufolens.com/api/v1). L ie fat de doi perts — na pipeline de ingest locala en Python (`pipeline/`) y na app de bòrd en TypeScript/Hono (`worker/`) — che se anconta te n'interfaza: n bundle publicà de SQL + assets.

Ne t'l ie debujen deguna credenziai de cloud per contribuì. I modui zentrei dla pipeline ie ma stdlib y i tesć dl Worker va contra na memoria in-memory.

### Met reva

```bash
# pipeline
python3 -m pytest pipeline/tests/          # dëssa dut unì vërt, zënza debujen de pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ulà che l'aiut ie plu de utl

**i18n / localisazion** — `worker/src/i18n/ui-strings.json` ie la surant di tesć de UI. Na revijion da pert de n rujenadù natif de un di locai nia nglëisc ie de gran valor: ciafà output de maschina nia bel, curijer problems de RTL/layout, mlieuré caji de confin tla negoziazion dla rujeneda.

**Qualità de OCR** — mlieura pre-elaborazion de veies scansions scrites a maschina dan l'OCR; na strutura de valutazion che cunfrontea l motor open-source cun l fallback de Tesseract sun plates de proa.

**Azessibilità** — audité les plates rendisdes (`worker/src/render/`) cun WCAG; l CSP ie strent (no `unsafe-inline`), perchël muessa les soluzions funzioné te chësc cheder.

**Ergonomia dla API** — `worker/src/routes/` — paginazion, filtri, descrizion OpenAPI, clients de ejëmpl.

**Robustëza dla pipeline** — deplu vies de degradazion zënza problems, miëur reporting dl prozes, caji de confin tl rilevamënt de delta (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ie l'indesc). Traduzions di documënc de proiet en nglëisc ie bënunides.

### Regules de basa

- Duc i percurses muessa ester relativs — l proiet dëssa pudëi unì spostà da na maschina a l'autra. Degun percurs assolut fissà tl codesc.
- No anjenëter na dependënza de pip a n modul *zentrel* dla pipeline. Stadies facoltatifs possa durà pachec facoltatifs, y muessa se degradé zënza problems sce chisc mancia.
- No nflui la maschina a stac ma inant — chël ie l limit massim di cosć.
- No anjenëter degun segn ufizial del guviern di Stac Unic y no anjenëter nia che revochëssa les redazions dla surant.
- Mudamënc al schema de D1 reverda **doi** files: `pipeline/lib/manifest_schema.sql` y `db/schema.sql`.
- Tesć cun codesc nuef. Messajes de commit convenzionai.

Liejer `CLAUDE.md` y `docs/20260511/00-*` danora, y pona giaurì n issue per scuté de velch de strutural dan l PR.

