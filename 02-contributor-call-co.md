# GitHub — Articulu 2 di 3 · Chjama à i cuntributori / "boni primi prublemi"

**Aduprà cum'è:** una Discussione appiccicata ("Cuntribuzione & boni primi prublemi") o una introduzione à CONTRIBUTING.md.
**Parolle chjave:** open source, cuntribuzione, bonu primu prublema, i18n, lucalizazione, OCR, Python, TypeScript, Vitest, pytest, accessibilità, UAP, dati aperti
**Ligami ipertestuali:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cuntribuisce à ufolens.com

[ufolens.com](https://www.ufolens.com) trasforma l'[archiviu PURSUE UAP](https://www.war.gov/ufo) di u Dipartimentu di a Guerra di i Stati Uniti in una piattaforma ricircabile è multilingua cù una [API publica](https://www.ufolens.com/api/v1). Hè cumpostu di duie metà — un pipeline d'ingestione lucale in Python (`pipeline/`) è un'app edge in TypeScript/Hono (`worker/`) — chì si scontranu in una sola interfaccia: un pacchettu SQL + assi publicatu.

Ùn avete bisognu di alcuna credenziale cloud per cuntribuisce. I moduli principali di u pipeline sò solu stdlib è i testi di u Worker funzionanu contr'à un almacenamentu in memoria.

### Stallazione

```bash
# pipeline
python3 -m pytest pipeline/tests/          # duverebbe esse tuttu verde, senza bisognu di pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Induve l'aiutu hè più utile

**i18n / lucalizazione** — `worker/src/i18n/ui-strings.json` hè a surghjente di e stringhe di l'interfaccia d'utilizatore. A revisione da un parlante nativu di qualsiasi lingua non inglese hè di grande valore: catturà traduzzioni automatiche goffe, risolve prublemi di layout/RTL, migliurà i casi limite di a negoziazione di lingua.

**Qualità di l'OCR** — un megliu pretrattamentu di vechji ducumenti scritti à macchina prima di l'OCR; un'imbracatura di valutazione chì paragona u mutore open-source cù u fallback Tesseract nantu à pagine campione.

**Accessibilità** — auditare e pagine rese (`worker/src/render/`) contr'à u WCAG; u CSP hè strettu (senza `unsafe-inline`), dunque e suluzioni devenu funziunà in quellu quadru.

**Ergunumia di l'API** — `worker/src/routes/` — paginazione, filtru, descrizzione OpenAPI, clienti d'esempiu.

**Robustezza di u pipeline** — più percorsi di degradazione graziosa, megliu rapportu di prugressu, casi limite di rilevazione di delta (`pipeline/lib/delta.py`).

**Ducumenti** — `docs/20260511/` (繁體中文; `00-*` hè l'indici). E traduzzioni di i ducumenti di cuncepimentu in inglese sò benvenute.

### Regule di basa

- Tutti i percorsi sò relativi — u prugettu deve esse purtabile trà e macchine. Nisun percorsu assolutu codificatu.
- Ùn aghjunghjite micca una dipendenza pip à un modulu *core* di u pipeline. E tappe opzionali ponu aduprà pacchetti opzionali, è devenu degradassi cun grazia senza elli.
- Ùn indebulite micca a macchina à stati forward-only — hè u tettu di i costi.
- Ùn aghjunghjite micca insegne ufficiali di u guvernu di i Stati Uniti, è ùn aghjunghjite nunda chì inverte e redazzioni surghjente.
- I cambiamenti di schema D1 toccanu **dui** schedarii: `pipeline/lib/manifest_schema.sql` è `db/schema.sql`.
- Testi cù u novu codice. Missaghji di commit cunvinziunali.

Leghjite prima `CLAUDE.md` è `docs/20260511/00-*`, poi aprite un prublema per discute qualsiasi cosa strutturale prima di u PR.
