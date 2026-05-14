# GitHub — Post 2 di 3 · Appello ai contributor / "good first issues"

**Uso:** una Discussione in evidenza ("Contributing & good first issues") o un'introduzione per CONTRIBUTING.md.
**Parole chiave:** open source, contributing, good first issue, i18n, localizzazione, OCR, Python, TypeScript, Vitest, pytest, accessibilità, UAP, open data
**Hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuire a ufolens.com

[ufolens.com](https://www.ufolens.com) trasforma l'[archivio PURSUE UAP](https://www.war.gov/ufo) del Dipartimento della Guerra degli Stati Uniti in una piattaforma ricercabile e multilingue con un'[API pubblica](https://www.ufolens.com/api/v1). È composto da due metà — una pipeline di ingestione locale in Python (`pipeline/`) e un'applicazione edge in TypeScript/Hono (`worker/`) — che si incontrano in un'unica interfaccia: un bundle pubblicato di SQL + asset.

Non sono necessarie credenziali cloud per contribuire. I moduli principali della pipeline sono solo stdlib e i test del Worker vengono eseguiti su uno storage in-memory.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # dovrebbe essere tutto verde, nessuna installazione con pip necessaria

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Dove l'aiuto è più utile

**i18n / localizzazione** — `worker/src/i18n/ui-strings.json` è la fonte delle stringhe dell'interfaccia utente. La revisione da parte di un madrelingua di qualsiasi locale non inglese è di grande valore: individuare traduzioni automatiche goffe, risolvere problemi di layout/RTL, migliorare i casi limite della negoziazione della lingua.

**Qualità dell'OCR** — un migliore pre-processing delle vecchie scansioni dattiloscritte prima dell'OCR; un sistema di valutazione per confrontare il motore open-source con il fallback Tesseract su pagine di esempio.

**Accessibilità** — audit delle pagine renderizzate (`worker/src/render/`) rispetto alle WCAG; la CSP è restrittiva (no `unsafe-inline`), quindi le soluzioni devono funzionare all'interno di tale vincolo.

**Ergonomia dell'API** — `worker/src/routes/` — paginazione, filtri, descrizione OpenAPI, client di esempio.

**Robustezza della pipeline** — percorsi di degradazione più eleganti, migliore reporting dello stato di avanzamento, casi limite nel rilevamento dei delta (`pipeline/lib/delta.py`).

**Documentazione** — `docs/20260511/` (繁體中文; `00-*` è l'indice). Le traduzioni in inglese dei documenti di progettazione sono benvenute.

### Regole di base

- Tutti i percorsi devono essere relativi — il progetto deve essere portabile tra macchine diverse. Nessun percorso assoluto hardcoded.
- Non aggiungere dipendenze pip a un modulo *principale* della pipeline. Le fasi opzionali possono utilizzare pacchetti opzionali e devono degradare elegantemente in loro assenza.
- Non indebolire la macchina a stati a solo avanzamento — quello è il tetto dei costi.
- Non introdurre insegne ufficiali del governo degli Stati Uniti e non aggiungere nulla che annulli le parti redatte nei documenti originali.
- Le modifiche allo schema D1 interessano **due** file: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Includere test con il nuovo codice. Messaggi di commit in formato Conventional Commits.

Leggi prima `CLAUDE.md` e `docs/20260511/00-*`, poi apri una issue per discutere qualsiasi modifica strutturale prima della PR.
