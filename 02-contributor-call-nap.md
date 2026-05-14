# GitHub — Post 2 ’e 3 · Chiamata a contribuì / "prime facenne bone"

**Ausà comme:** na Discussione appuntata ("Cuntribbuì e prime facenne bone") o n'introduzzione a CONTRIBUTING.md.
**Parole chiave:** open source, cuntribbuì, prima facenna bona, i18n, lucalizzazzione, OCR, Python, TypeScript, Vitest, pytest, accessibbeletà, UAP, date aperte
**Culligamente:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cuntribbuì a ufolens.com

[ufolens.com](https://www.ufolens.com) trasforma ll'archivio [PURSUE UAP](https://www.war.gov/ufo) d’ ’o Dipartimento d’ ’a Guerra d’ ’e State Aunite ’int’a na piattaforma cercabbile e multilingua cu n'[API pubbreca](https://www.ufolens.com/api/v1). So’ doje mità — nu pipeline d'ingestione locale ’n Python (`pipeline/`) e n'app edge ’n TypeScript/Hono (`worker/`) — ca s'incontrano a n'interfaccia sola: nu pacchetto pubbrecato ’e SQL + assets.

Nun v’ servono credenziale cloud pe cuntribbuì. ’E module principale d’ ’o pipeline so’ solo stdlib e ’e teste d’ ’o Worker girano cuntro a nu storage ’n memoria.

### Messa a punto

```bash
# pipeline
python3 -m pytest pipeline/tests/          # avessa ì tutto buono, senza bisogno ’e pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Addó ll’ajuto serve ’e cchiù

**i18n / lucalizzazzione** — `worker/src/i18n/ui-strings.json` è ’a surgenta d’ ’e stringhe d’ ’a UI. Na revisione fatta ’a chi parla nativamente na lengua ca nun è ll’inglese tene nu granne valore: beccà traduzzione machine-made malamente, sistemà prubleme cu RTL/layout, migliorà ’e case limite d’ ’a negoziazzione d’ ’a lengua.

**Qualità d’ ’o OCR** — na pre-elaborazzione megliore d’ ’e vecchie scanzione scritte a machina primma d’ ’o OCR; nu sistema ’e valutazione ca paragona ’o motore open-source cuntro ’o fallback Tesseract ncopp’a paggene ’e prova.

**Accessibbeletà** — verificà ’e paggene renderizzate (`worker/src/render/`) cuntro ’e WCAG; ’o CSP è rigido (nisciun `unsafe-inline`), quinni ’e soluzzione hann’ ’a funzionà rispettannolo.

**Ergonomia d’ ’a API** — `worker/src/routes/` — pagginazzione, filtraggio, descrizzione OpenAPI, clienti d’esempio.

**Robustezza d’ ’o pipeline** — cchiù percurse ’e degradazzione graziosa, megliore reporting d’ ’o pruciesso, case limite d’ ’a rilevazzione d’ ’e delta (`pipeline/lib/delta.py`).

**Ducumentazzione** — `docs/20260511/` (繁體中文; `00-*` è ll'indice). ’E traduzzione d’ ’e ducumente ’e design ’n inglese so’ benvenute.

### Regole ’e base

- Tutte ’e percurse so’ relative — ’o pruggetto s’adda puté purtà ncopp’a machina diverse. Nisciun percurso assoluto scritto a penna.
- Nun aggiugnere na dipendenza pip a nu modulo *core* d’ ’o pipeline. ’E fase opzionale ponno ausà pacchette opzionale, e hann’ ’a degradà cu grazia senza ’e chiste.
- Nun ’ndebulì ’a machina a state ca va solo ’nnante — chillo è ’o tetto d’ ’o costo.
- Nun ’ntroducere simbole ufficiale d’ ’o guvierno d’ ’e State Aunite, e nun aggiungere niente ca cancella ’e ridazzione d’ ’a surgenta.
- ’E cagnamiente d’ ’o schema D1 toccano **doje** file: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Teste cu ’o codice nuovo. Messagge ’e commit convenzionale.

Leggete `CLAUDE.md` e `docs/20260511/00-*` primma, e po’ aprite n'issue pe discorrere ’e qualunque cosa strutturale primma d’ ’a PR.

