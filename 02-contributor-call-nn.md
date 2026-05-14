# GitHub – Innlegg 2 av 3 · Oppfordring til bidrag / «gode første saker»

**Bruk som:** ein festa diskusjon («Bidrag og gode første saker») eller ein introduksjon i CONTRIBUTING.md.
**Nøkkelord:** open source, bidra, god første sak, i18n, lokalisering, OCR, Python, TypeScript, Vitest, pytest, tilgjenge, UAP, opne data
**Hyperlenker:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bidra til ufolens.com

[ufolens.com](https://www.ufolens.com) gjer det amerikanske krigsdepartementets [PURSUE UAP-arkiv](https://www.war.gov/ufo) om til ein søkbar, fleirspråkleg plattform med ein [offentleg API](https://www.ufolens.com/api/v1). Det er to halvdelar – ein lokal Python-inntaksrøyrleidning (`pipeline/`) og ein TypeScript/Hono edge-app (`worker/`) – som møtast ved eitt grensesnitt: ein publisert SQL + ressurs-pakke.

Du treng ingen sky-legitimasjon for å bidra. Røyrleidningskjernemodulane er berre-stdlib, og Worker-testane køyrer mot minnebasert lagring.

### Oppsett

```bash
# pipeline
python3 -m pytest pipeline/tests/          # bør vere heilt grøn, ingen pip install nødvendig

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kvar hjelp er mest nyttig

**i18n / lokalisering** – `worker/src/i18n/ui-strings.json` er kjelda til UI-strengar. Morsmålsgjennomgang av alle ikkje-engelske lokaliseringar er av høg verdi: fang opp klønete maskinomsetjing, fiks RTL/layout-problem, forbetre kantsaker i språkforhandling.

**OCR-kvalitet** – betre førehandsbehandling av gamle maskinskrivne skanningar før OCR; evalueringssele som samanliknar open-source-motoren mot Tesseract-fallback på prøvesider.

**Tilgjenge** – revider dei rendererte sidene (`worker/src/render/`) mot WCAG; CSP-en er streng (ingen `unsafe-inline`), så løysingar må fungere innanfor det.

**API-ergonomi** – `worker/src/routes/` – paginering, filtrering, OpenAPI-skildring, døme-klientar.

**Røyrleidningsrobustheit** – fleire grasiøse degraderingsstiar, betre framdriftsrapportering, kantsaker i delta-deteksjon (`pipeline/lib/delta.py`).

**Dokumentasjon** – `docs/20260511/` (繁體中文; `00-*` er indeksen). Omsetjingar av designdokumenta til engelsk er velkomne.

### Grunnreglar

- Alle stiar relative – prosjektet må vere portabelt på tvers av maskiner. Ingen hardkoda absolutte stiar.
- Ikkje legg til ein pip-avhengnad til ein *kjerne*-modul i røyrleidninga. Valfrie steg kan bruke valfrie pakkar, og må degradere grasiøst utan dei.
- Ikkje svekk den berre-framover-tilstandsmaskina – det er kostnadstaket.
- Ikkje introduser offisielle amerikanske regjeringsemblem, og ikkje legg til noko som reverserer kjelde-sladdingar.
- D1-skjemaendringar rører ved **to** filer: `pipeline/lib/manifest_schema.sql` og `db/schema.sql`.
- Testar med ny kode. Conventional-commit-meldingar.

Les `CLAUDE.md` og `docs/20260511/00-*` først, opne deretter ein issue for å diskutere noko strukturelt før PR-en.

