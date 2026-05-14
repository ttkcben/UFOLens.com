# GitHub — Post 2 di 3 · Clamade ai colaboradôrs / "prins lavôrs facii"

**Ús come:** une discussion fissade ("Contribuî & prins lavôrs facii") o une introduzion a CONTRIBUTING.md.
**Perpaulis clâf:** open source, contribuî, prin lavôr facil, i18n, localizazion, OCR, Python, TypeScript, Vitest, pytest, acessibilitât, UAP, dâts vierts
**Leams ipertestuâi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuî a ufolens.com

[ufolens.com](https://www.ufolens.com) al trasforme l'[archivi PURSUE UAP](https://www.war.gov/ufo) dal Dipartiment de Vuere dai Stâts Unîts intune plateforme ricercjabil e multilengâl cuntune [API publiche](https://www.ufolens.com/api/v1). Al è fat di dôs parts — une pipeline di ingestion in Python locâl (`pipeline/`) e une aplicazion edge in TypeScript/Hono (`worker/`) — che si incuintrin intune sole interface: un pachet publicât di SQL + assets.

No tu âs bisugne di credenziâi cloud par contribuî. I modui cûr de pipeline a son dome stdlib e i tescj dal Worker a funzionin cuntune memorie in-memory.

### Configurazion

```bash
# pipeline
python3 -m pytest pipeline/tests/          # dut al varès di jessi vert, cence bisugne di instalâ vie pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Dulà che l'aiût al è plui util

**i18n / localizazion** — `worker/src/i18n/ui-strings.json` al è la sorzint des stringhis de interface utent. La revision di un madrelenghe di cualsisei lenghe no inglese e je di grant valôr: par cjatâ risultâts di machine stramp, par sistemâ problemis di RTL/layout, par miorâ i câs limits de negoziazion de lenghe.

**Qualitât OCR** — un miôr pre-procesament des vecjis scansions a machine di scrivi prime dal OCR; un framework di valutazion che al confronte il motôr open-source cul fallback Tesseract su pagjinis di campion.

**Acessibilitât** — verificâ lis pagjinis generadis (`worker/src/render/`) cuintri lis normis WCAG; la CSP e je rigurose (nissun `unsafe-inline`), duncje lis soluzions a devin funzionâ dentri di chel limit.

**Ergonomie des API** — `worker/src/routes/` — pagjinazion, filtradi, descrizion OpenAPI, clients di esempli.

**Robustece de pipeline** — plui percors di degradazion elegante, miôr segnalazion dal progres, câs limits te rilevazion des diferencis (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` al è l'indiç). Lis traduzions dai documents di progjet par inglês a son benvignudis.

### Regulis di base

- Ducj i percors a devin jessi relatîfs — il progjet al à di podê jessi spostât tra machinis diviersis. Nissun percors assolût hardcoded.
- No zontâ dependencis pip a un modul cûr de pipeline. Lis fasis opzionâls a puedin doprâ pachets opzionâi, e a devin degradâ cun grâce cence di lôr.
- No indebolî la machine a stâts dome indevant — chel al è il tet di spese.
- No introduî insegnes uficiâls dal guvier dai Stâts Unîts, e no zontâ nuie che al gjavi lis redazions de sorzint.
- Lis modifichis al schem D1 a tocjin **doi** files: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Tescj cul gnûf codis. Messaçs di commit convenzionâi.

Lieç prime `CLAUDE.md` e `docs/20260511/00-*`, e dopo vierç une segnalazion par discuti cualsisei cambiament struturâl prime di fâ une PR.

