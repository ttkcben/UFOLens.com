# GitHub — Post 2 de 3 · Tzerriada a contribuire / "primas issues bonas"

**Impreu comente:** una Diskussione fissada ("Cun contribuire & primas issues bonas") o un'introduida a CONTRIBUTING.md.
**Faeddos crae:** open source, contribuire, prima issue bona, i18n, localizatzione, OCR, Python, TypeScript, Vitest, pytest, atzessibilidade, UAP, datos abertos
**Acàpios:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cun contribuire a ufolens.com

[ufolens.com](https://www.ufolens.com) trasformat s'archiviu [PURSUE UAP de su Dipartimentu de sa Gherra de is Istados Unidos](https://www.war.gov/ufo) in una prataforma chircàbile e multilìngua cun una [API pùblica](https://www.ufolens.com/api/v1). Sunt duas metades — una pipeline de achirimentu in Python locale (`pipeline/`) e un'aplicatzione edge in TypeScript/Hono (`worker/`) — chi s'addòbiant in un'interfache sceti: unu pachete publicadu de SQL + assets.

No serbit peruna credentziale de su cloud pro contribuire. Is mòdulos de su nùcleu de sa pipeline sunt sceti stdlib e is proas de su Worker funtzionant cun una memòria in-memory.

### Configuratzione

```bash
# pipeline
python3 -m pytest pipeline/tests/          # diat dèpere èssere totu birde, sena instalare nudda cun pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ue s'agiudu serbit de prus

**i18n / localizatzione** — `worker/src/i18n/ui-strings.json` est s'origine de is stringas de s'UI. Una revisione fata dae unu faeddante nadiu de cale si siat limba chi no est s'inglesu est de valore mannu: agatare tradutziones automàticas pagu naturales, acontzare problemas de RTL/layout, megiorare casos lìmites in sa negotziatzione de sa limba.

**Calidade de s'OCR** — megiorare su pre-protzessamentu de is documentos antigos iscritzionados a màchina in antis de s'OCR; creare un'ambiente de proa pro cumparare su motore open-source cun s'alternativa Tesseract in pàginas de proa.

**Atzessibilidade** — auditare is pàginas visualizadas (`worker/src/render/`) cun is critèrios WCAG; su CSP est tostu (perunu `unsafe-inline`), duncas is solutziones depent funtzionare in intro de cussos lìmites.

**Ergonomia de s'API** — `worker/src/routes/` — paginatzione, filtràgiu, descritzione OpenAPI, esèmpios de clientes.

**Robustesa de sa pipeline** — prus maneras de s'adatare cando calchi cosa faddit, megiorare is sinnales de progressu, gestire casos lìmites in su rilevamentu de is delta (`pipeline/lib/delta.py`).

**Documentos** — `docs/20260511/` (繁體中文; `00-*` est s'ìnditze). Is tradutziones de is documentos de progetu in inglesu sunt bene bènnidas.

### Règulas de base

- Totu is percursos depent èssere relativos — su progetu depet èssere portàbile intre màchinas. Perunu percursu assolutu fissu in su còdighe.
- No agiungas una dipendèntzia de pip a unu mòdulu de su *nùcleu* de sa pipeline. Is fases optzionales podent impreare pachetes optzionales, e depent funtzionare in manera curreta etotu si custos mancant.
- No indebilites sa màchina a istados chi andat sceti a in antis — cussu est su lìimite de is costos.
- No agiungas sìmbulos ufitziales de su guvernu de is Istados Unidos, e no agiungas nudda chi potzat isvelare is partes testuales eliminadas in is documentos originales.
- Is càmbios a s'ischema de D1 tocant **duos** files: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Proas cun còdighe nou. Messàgios de commit cun su formadu Conventional-commit.

Lege `CLAUDE.md` e `docs/20260511/00-*` in antis, a pustis aberi una issue pro discutire cale si siat càmbio istruturale in antis de fàghere sa PR.

