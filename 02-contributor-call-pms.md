# GitHub — Publicassion 2 ëd 3 · Ciamà për contributor / "bon prim problema"

**Dovré com:** na Discussion fërmà ("Contribuì e bon prim problema") o n'antrodussion a CONTRIBUTING.md.
**Paròle ciav:** open source, contribussion, bon prim problema, i18n, localisassion, OCR, Python, TypeScript, Vitest, pytest, acessibilità, UAP, dàit duvert
**Anliure:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuì a ufolens.com

[ufolens.com](https://www.ufolens.com) a trasforma l'archiv [PURSUE UAP](https://www.war.gov/ufo) dël Dipartiment dla Guèra djë Stat Unì an na piataforma sërcàbil e multilingua con n'[API pùblica](https://www.ufolens.com/api/v1). A l'é fàit ëd doe mità — na pipeline d'ingestion local an Python (`pipeline/`) e n'aplicassion edge an TypeScript/Hono (`worker/`) — ch'a s'ancontro an n'interfacia sola: un pachet publicà SQL + asset.

A l'é pa necesari avèj credensiaj cloud për contribuì. Ij mòduj prinsipaj dla pipeline a son mach stdlib e ij test dël Worker a giro contra na memòria an memòria.

### Anstalassion

```bash
# pipeline
python3 -m pytest pipeline/tests/          # a dovrìa esse tut verd, gnun-a instalassion con pip a l'é necesari

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Andoa che l'agiut a l'é pì util

**i18n / localisassion** — `worker/src/i18n/ui-strings.json` a l'é la sorgiss dle stringhe dl'interfacia utent. La revision da part ëd parlant nativ ëd minca localisassion nen anglèisa a l'ha un gran valor: corege j'output automàtich nen naturaj, rangé ij problema ëd RTL/layout, mijoré ij cas lìmit dla negossiassion dla lenga.

**Qualità dl'OCR** — un pre-processament mijor djë scansion ëd document vej batù a màchina prima dl'OCR; n'ambragadura ëd valutassion ch'a confronta ël motor open-source con la riserva Tesseract su pàgine campion.

**Acessibilità** — verifiché le pàgine generà (`worker/src/render/`) contra le WCAG; ël CSP a l'é sever (gnun `unsafe-inline`), donca le solussion a devo fonsioné an cost contest.

**Ergonomìa dl'API** — `worker/src/routes/` — paginassion, filtrage, descrission OpenAPI, client d'esempi.

**Robustëssa dla pipeline** — pì percors ëd degradassion grassiosa, mijor rendicontassion dël progress, cas lìmit ant la detession dij delta (`pipeline/lib/delta.py`).

**Documentassion** — `docs/20260511/` (繁體中文; `00-*` a l'é l'ìndes). Le tradussion dij document ëd proget an anglèis a son bin accetà.

### Régole ëd base

- Tuti ij senté a son relativ — ël proget a dev esse portàbil tra le màchine. Gnun senté assolù codificà.
- Gionté pa na dipendensa pip a un mòdul *prinsipal* dla pipeline. Jë stadi opsionaj a peulo dovré pachet opsionaj, e a devo degragé con grassia sensa.
- Andebolì pa la màchina a stat mach anans — col a l'é ël lìmit massimal ëd cost.
- Gionté pa d'ansigne ofissiaj dël goern djë Stat Unì, e gionté pa gnente ch'a anula le redassion sorgiss.
- Le modìfiche al schema D1 a toco **doi** file: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Test con còdes neuv. Mëssagi ëd commit conform a Conventional Commits.

Lese `CLAUDE.md` e `docs/20260511/00-*` prima, peui duverté un problema për discute qualonque ròba strutural prima dla PR.

