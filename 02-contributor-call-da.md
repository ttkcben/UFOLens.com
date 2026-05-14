# GitHub — Indlæg 2 af 3 · Opfordring til bidragydere / "gode første opgaver"

**Anvendelse:** En fastgjort diskussion ("Bidrag & gode første opgaver") eller en introduktion til CONTRIBUTING.md.
**Nøgleord:** open source, bidrag, god første opgave, i18n, lokalisering, OCR, Python, TypeScript, Vitest, pytest, tilgængelighed, UAP, åbne data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bidrag til ufolens.com

[ufolens.com](https://www.ufolens.com) omdanner det amerikanske krigsministeriums [PURSUE UAP-arkiv](https://www.war.gov/ufo) til en søgbar, flersproget platform med en [offentlig API](https://www.ufolens.com/api/v1). Det består af to halvdele — en lokal Python-indtagelsespipeline (`pipeline/`) og en TypeScript/Hono edge-app (`worker/`) — der mødes ved én grænseflade: et udgivet SQL + aktiv-bundt.

Du behøver ingen cloud-legitimationsoplysninger for at bidrage. Pipelinens kernemoduler er kun afhængige af stdlib, og Worker-testene kører mod in-memory lager.

### Opsætning

```bash
# pipeline
python3 -m pytest pipeline/tests/          # bør alle være grønne, ingen pip-installation nødvendig

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Hvor hjælp er mest nyttig

**i18n / lokalisering** — `worker/src/i18n/ui-strings.json` er kilden til UI-strenge. Gennemgang af enhver ikke-engelsk lokalitet af en modersmålstalende er af høj værdi: find akavet maskinoversat output, ret RTL/layout-problemer, forbedr edge cases for sprogforhandling.

**OCR-kvalitet** — bedre forbehandling af gamle maskinskrevne scanninger før OCR; evalueringsværktøj, der sammenligner open source-motoren med Tesseract-fallback på eksempelsider.

**Tilgængelighed** — auditér de renderede sider (`worker/src/render/`) mod WCAG; CSP'en er streng (ingen `unsafe-inline`), så løsninger skal fungere inden for den ramme.

**API-ergonomi** — `worker/src/routes/` — paginering, filtrering, OpenAPI-beskrivelse, eksempelklienter.

**Pipeline-robusthed** — flere elegante nedbrydningsstier, bedre statusrapportering, edge cases for delta-detektion (`pipeline/lib/delta.py`).

**Dokumentation** — `docs/20260511/` (繁體中文; `00-*` er indekset). Oversættelser af design-dokumenterne til engelsk er velkomne.

### Grundregler

- Alle stier er relative — projektet skal kunne flyttes mellem maskiner. Ingen hårdkodede absolutte stier.
- Tilføj ikke en pip-afhængighed til et pipeline-*kerne*-modul. Valgfrie stadier må bruge valgfrie pakker og skal nedbrydes elegant uden dem.
- Svæk ikke den kun-fremadrettede tilstandsmaskine — det er omkostningsloftet.
- Introducer ikke officielle amerikanske regerings-emblemer, og tilføj ikke noget, der fjerner kildens redaktioner.
- D1-skemaændringer berører **to** filer: `pipeline/lib/manifest_schema.sql` og `db/schema.sql`.
- Tests med ny kode. Conventional-commit-beskeder.

Læs `CLAUDE.md` og `docs/20260511/00-*` først, og åbn derefter et issue for at diskutere noget strukturelt før en PR.

