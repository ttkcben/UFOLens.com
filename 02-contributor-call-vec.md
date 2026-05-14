# GitHub — Post 2 de 3 · Ciamada a contribuir / "boni primi compiti"

**Doparar come:** na Discussion fisà in alto ("Contribuir e boni primi compiti") o n'introdusion a CONTRIBUTING.md.
**Parole ciave:** open source, contribuir, boni primi compiti, i18n, localixasion, OCR, Python, TypeScript, Vitest, pytest, acesibilità, UAP, dati verti
**Colegamenti ipertestuałi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuir a ufolens.com

[ufolens.com](https://www.ufolens.com) el trasforma l'archivio [PURSUE UAP](https://www.war.gov/ufo) del Dipartimento de ła Guera dei Stati Unii in na piataforma sercàbiłe e multilingue co na [API publica](https://www.ufolens.com/api/v1). El xe fato de do metà — un pipeline de ingesting local in Python (`pipeline/`) e n'aplicasion edge in TypeScript/Hono (`worker/`) — che łe se incontra so n'interfacia soła: un bundle publicà de SQL + asset.

No serve nesuna credensial del cloud par contribuir. I moduli core del pipeline i dopara soło la libraria stàndar e i test del Worker i xira so un storage in memoria.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # dovaria èsar tuto verde, sensa bisogno de instalar co pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Dove che l'aiuto el serve de pì

**i18n / localixasion** — `worker/src/i18n/ui-strings.json` el xe la fonte de łe stringhe de l'interfacia utente. Na revision da parte de un marelengua de qualsiasi łocałe no inglexe ła xe de gran valor: catar tradusion automatiche fate małe, sistemar problemi de RTL/layout, meiorar i caxi limite de ła negosiasion de ła łengua.

**Qualità de l'OCR** — meiorar el pre-processing de łe vece scansioni scrite a machina prima de l'OCR; un sistema de valutasion che confronta el motore open-source col Tesseract de riserva so pagine de exenpio.

**Acesibilità** — far un audit de łe pagine generà (`worker/src/render/`) contro łe WCAG; el CSP el xe severo (nesun `unsafe-inline`), quindi łe solusion łe ga da funsionar drento sti limiti.

**Ergonomia de l'API** — `worker/src/routes/` — paginasion, filtri, descrision OpenAPI, client de exenpio.

**Robustesa del pipeline** — pì percorsi de degradason grasioxa, meior reporting del progresso, caxi limite nel rilevamento dei delta (`pipeline/lib/delta.py`).

**Documentasion** — `docs/20260511/` (繁體中文; `00-*` el xe l'index). Łe tradusion dei documenti de progeto in inglexe łe xe benvegnùe.

### Regołe de base

- Tuti i percorsi i ga da èsar relativi — el projeto el ga da podarse spostar tra machine. Nesun percorso asoluto hardcoded.
- No xontar na dipendensa da pip a un modulo *core* del pipeline. Fasi opsionałi łe pol doparar pacheti opsionałi, e łe ga da degradarse co grasia sensa de lori.
- No indebołir la machina a stati soło in vanti — queło el xe el nostro teto de costo.
- No xontar stemi ufisiai del governo dei Stati Unii, e no xontar gnente che desfa łe censure fonte.
- I canbiamenti al schema D1 i toca **do** file: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Test col codexe novo. Mesaji de commit Convensionałi.

Lexi prima `CLAUDE.md` e `docs/20260511/00-*`, po' vèrxi n'issue par discorer de qualsiasi roba strutural prima de far la PR.

