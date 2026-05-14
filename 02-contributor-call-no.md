# GitHub – Innlegg 2 av 3 · Oppfordring til bidragsytere / "gode første saker"

**Brukes som:** en festet diskusjon ("Bidrag og gode første saker") eller en introduksjon i CONTRIBUTING.md.
**Nøkkelord:** åpen kildekode, bidra, god første sak, i18n, lokalisering, OCR, Python, TypeScript, Vitest, pytest, tilgjengelighet, UAP, åpne data
**Hyperlenker:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bidra til ufolens.com

[ufolens.com](https://www.ufolens.com) gjør det amerikanske krigsdepartementets [PURSUE UAP-arkiv](https://www.war.gov/ufo) om til en søkbar, flerspråklig plattform med et [offentlig API](https://www.ufolens.com/api/v1). Det består av to halvdeler – en lokal Python-ingest-pipeline (`pipeline/`) og en TypeScript/Hono edge-app (`worker/`) – som møtes ved ett grensesnitt: en publisert SQL + ressurs-pakke.

Du trenger ingen sky-autentisering for å bidra. Pipelinens kjernemoduler er kun avhengige av stdlib, og Worker-testene kjører mot minneintern lagring.

### Oppsett

```bash
# pipeline
python3 -m pytest pipeline/tests/          # skal være helt grønn, ingen pip-installasjon nødvendig

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Hvor hjelp er mest nyttig

**i18n / lokalisering** — `worker/src/i18n/ui-strings.json` er kilden til UI-strenger. En gjennomgang av en hvilken som helst ikke-engelsk lokalisering av en morsmålstaler er av høy verdi: finn klønete maskinoversatt resultat, fiks RTL-/layout-problemer, forbedre unntakstilfeller i språkforhandling.

**OCR-kvalitet** — bedre forbehandling av gamle, maskinskrevne skanninger før OCR; et evalueringsrammeverk som sammenligner open source-motoren med Tesseract-fallbacken på eksempelsider.

**Tilgjengelighet** — revider de renderte sidene (`worker/src/render/`) mot WCAG; CSP-en er streng (ingen `unsafe-inline`), så løsninger må fungere innenfor disse rammene.

**API-ergonomi** — `worker/src/routes/` — paginering, filtrering, OpenAPI-beskrivelse, eksempelklienter.

**Robusthet i pipeline** — flere veier for elegant degradering, bedre fremdriftsrapportering, unntakstilfeller i deltadeteksjon (`pipeline/lib/delta.py`).

**Dokumentasjon** — `docs/20260511/` (繁體中文; `00-*` er indeksen). Oversettelser av designdokumentene til engelsk er velkomne.

### Grunnregler

- Alle stier skal være relative — prosjektet må være portabelt på tvers av maskiner. Ingen hardkodede absolutte stier.
- Ikke legg til en pip-avhengighet i en *kjerne*-modul i pipelinen. Valgfrie steg kan bruke valgfrie pakker, men må degradere elegant uten dem.
- Ikke svekk enveis-tilstandsmaskinen — det er kostnadstaket.
- Ikke introduser offisielle amerikanske regjeringsinsignier, og ikke legg til noe som reverserer kildesladding.
- D1-skjemaendringer berører **to** filer: `pipeline/lib/manifest_schema.sql` og `db/schema.sql`.
- Tester må følge med ny kode. Bruk Conventional Commits-meldinger.

Les `CLAUDE.md` og `docs/20260511/00-*` først, og åpne deretter en sak for å diskutere større strukturelle endringer før du lager en PR.
