# GitHub — Post 2 de 3 · Ciamada ai contributor / "bon prim problema"

**Doperà 'me:** una Discussion fissada ("Come Contribuì & bon prim problema") o 'me introduzzion a CONTRIBUTING.md.
**Paròle ciav:** open source, contribuì, bon prim problema, i18n, localizazzion, OCR, Python, TypeScript, Vitest, pytest, acessibilità, UAP, dacc avercc
**Colegamencc ipertestuai:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Come Contribuì a ufolens.com

[ufolens.com](https://www.ufolens.com) el trasforma l'archivi [PURSUE UAP](https://www.war.gov/ufo) del Dipartiment de la Guerra di Stacc Unicc in d'una piataforma con fonzion de rezercha, multilengh e con una [API publica](https://www.ufolens.com/api/v1). L'è fad su de do part — una pipeline local de ingesta in Python (`pipeline/`) e una aplicazzion edge in TypeScript/Hono (`worker/`) — che se incontren in d'una interfacia: un pachet publicad de SQL + asset.

Te gh'heet minga besogn de credenziai cloud per dà una man. I modui del còrp de la pipeline i enn domà stdlib e i test del Worker i vann contra un stocagg in memoria.

### Misa in fonzion

```bash
# pipeline
python3 -m pytest pipeline/tests/          # el dovraria vesser tut verd, senza besogn de installà con pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Indove l'è che l'ajutt el serv pussee

**i18n / localizazzion** — `worker/src/i18n/ui-strings.json` l'è la fœn di stringhe de l'interfaccia utent. La revision de un parlant nativ per ogni localizazzion che l'è no in ingles l'è de gran valor: per becà di traduzzion automate un poo brute, sistemà problema de RTL/disposizzion, e mejorà i cas limicc de la negoziazzion de la lengua.

**Qualità de l'OCR** — una pre-elaborazzion pussee bona di vece scansion scrite a machina prima de l'OCR; un sistema de valutazzion che el confronta el motor open-source con el fallback Tesseract in su di pagine de esempi.

**Acessibilità** — fà un audit di pagine renderizade (`worker/src/render/`) contra i WCAG; el CSP l'è sever (nissun `unsafe-inline`), donca i soluzzion i gh'hann de fonzionà con 'sto limit.

**Ergonomia de l'API** — `worker/src/routes/` — paginazzion, filtragg, descrizzion OpenAPI, cliencc de esempi.

**Robustezza de la pipeline** — pussee percors de degradazzion con grazia, segnalazzion del progress pussee bona, cas limicc in de la rilevazzion di delta (`pipeline/lib/delta.py`).

**Documencc** — `docs/20260511/` (繁體中文; `00-*` l'è l'index). I traduzzion di documencc de progetazzion in ingles i enn benvegnude.

### Regole de bas

- Tœucc i percors i gh'ha de vesser relativ — el projet el gh'ha de podé vesser portad in su machine diferente. Nissun percors assolut codificad.
- Sgiontà no una dipendenza de pip a un modul del *còrp* de la pipeline. I fasi opzionai i pœden doperà pachecc opzionai, e i gh'hann de degradà con grazia senza de lor.
- Indebolì no la machina a stacc che la va domà inanz — quell l'è el tecc di coscc.
- Sgiontà no insegne ofizziai del govern di Stacc Unicc, e sgiontà no robe che i caven via i test scondœucc di fœn.
- I cambiamencc al schema de D1 i tochen **du** file: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Test con còdes nœuv. Messagg de commit in stil Conventional-commit.

Lensg `CLAUDE.md` e `docs/20260511/00-*` prima, e poeu dervì un problema per discorrer de cualquier roba strutural prima de la PR.

