# GitHub – Inlägg 2 av 3 · Uppmaning till bidrag / "bra första issues"

**Använd som:** en fäst diskussion ("Bidrag & bra första issues") eller en introduktion i CONTRIBUTING.md.
**Nyckelord:** öppen källkod, bidra, bra första issue, i18n, lokalisering, OCR, Python, TypeScript, Vitest, pytest, tillgänglighet, UAP, öppen data
**Hyperlänkar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bidra till ufolens.com

[ufolens.com](https://www.ufolens.com) omvandlar det amerikanska krigsdepartementets [PURSUE UAP-arkiv](https://www.war.gov/ufo) till en sökbar, flerspråkig plattform med ett [offentligt API](https://www.ufolens.com/api/v1). Det består av två halvor – en lokal Python-ingestionspipeline (`pipeline/`) och en TypeScript/Hono edge-app (`worker/`) – som möts i ett enda gränssnitt: ett publicerat paket med SQL + tillgångar.

Du behöver inga moln-inloggningsuppgifter för att bidra. Pipelinens kärnmoduler är endast beroende av standardbiblioteket och Worker-testerna körs mot ett minnesbaserat lagringsutrymme.

### Installation

```bash
# pipeline
python3 -m pytest pipeline/tests/          # allt bör vara grönt, ingen pip-installation behövs

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Var hjälp är mest användbar

**i18n / lokalisering** – `worker/src/i18n/ui-strings.json` är källan för UI-strängar. Granskning av en modersmålstalare för alla icke-engelska språkversioner är av högt värde: upptäcka klumpiga maskinöversättningar, fixa RTL/layoutproblem, förbättra kantfall i språkförhandling.

**OCR-kvalitet** – bättre förbehandling av gamla maskinskrivna skanningar före OCR; en utvärderingssele som jämför den öppen källkods-baserade motorn med Tesseract-reserven på exempelsidor.

**Tillgänglighet** – granska de renderade sidorna (`worker/src/render/`) mot WCAG; CSP är strikt (inget `unsafe-inline`), så lösningar måste fungera inom dessa ramar.

**API-ergonomi** – `worker/src/routes/` – paginering, filtrering, OpenAPI-beskrivning, exempelklienter.

**Pipeline-robusthet** – fler vägar för elegant degradering, bättre förloppsrapportering, kantfall för deltadetektering (`pipeline/lib/delta.py`).

**Dokumentation** – `docs/20260511/` (繁體中文; `00-*` är indexet). Översättningar av designdokumenten till engelska är välkomna.

### Grundregler

- Alla sökvägar relativa – projektet måste vara portabelt mellan maskiner. Inga hårdkodade absoluta sökvägar.
- Lägg inte till ett pip-beroende i en *kärnmodul* i pipelinen. Valfria steg kan använda valfria paket och måste degradera elegant utan dem.
- Försvaga inte den framåtriktade tillståndsmaskinen – det är kostnadstaket.
- Introducera inte officiella amerikanska regeringsinsignier och lägg inte till något som återställer källredigeringar.
- D1-schemaändringar påverkar **två** filer: `pipeline/lib/manifest_schema.sql` och `db/schema.sql`.
- Tester med ny kod. Konventionella commit-meddelanden.

Läs `CLAUDE.md` och `docs/20260511/00-*` först, öppna sedan ett issue för att diskutera eventuella strukturella ändringar innan du skapar en PR.
