# GitHub — Post 2 fan 3 · Oprop foar bydragen / "goede earste issues"

**Brûk as:** in fêstmakke Diskusje ("Bydrage & goede earste issues") of in `CONTRIBUTING.md`-yntro.
**Kaaiwurden:** iepen boarne, bydrage, goede earste issue, i18n, lokalisaasje, OCR, Python, TypeScript, Vitest, pytest, tagonklikens, UAP, iepen data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bydrage oan ufolens.com

[ufolens.com](https://www.ufolens.com) makket fan it [PURSUE UAP-argyf](https://www.war.gov/ufo) fan it Amerikaanske Oarlochsdepartemint in sykjeber, meartalich platfoarm mei in [publike API](https://www.ufolens.com/api/v1). It bestiet út twa helten — in lokale Python-ynfierpipeline (`pipeline/`) en in TypeScript/Hono edge-app (`worker/`) — dy't gearkomme by ien ynterface: in publisearre SQL + assets-bondel.

Jo hawwe gjin wolk-credentials nedich om by te dragen. De kearnmodules fan de pipeline binne allinich-stdlib en de Worker-tests rinne tsjin in-memory opslach.

### Opsetten

```bash
# pipeline
python3 -m pytest pipeline/tests/          # moat allegear grien wêze, gjin pip-ynstallaasje nedich

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Wêr't help it nuttichst is

**i18n / lokalisaasje** — `worker/src/i18n/ui-strings.json` is de boarne fan UI-strings. In kontrôle troch in memmetaalsprekker fan elke net-Ingelske lokaasje is fan hege wearde: nuvere masine-útfier opspoare, RTL/layout-problemen oplosse, rângefaltsjes by taalûnderhanneling ferbetterje.

**OCR-kwaliteit** — bettere foarferwurking fan âlde typte skens foarôfgeand oan OCR; in evaluaasje-harnas dat de iepenboarne-motor fergeliket mei de Tesseract-fallback op stekproefsiden.

**Tagonklikens** — kontrolearje de render-siden (`worker/src/render/`) tsjin WCAG; de CSP is strang (gjin `unsafe-inline`), dus oplossings moatte dêrbinnen wurkje.

**API-ergonomy** — `worker/src/routes/` — paginaasje, filterjen, OpenAPI-beskriuwing, foarbyld-kliïnten.

**Pipeline-robuustheid** — mear paden foar sierlik weromfallen, bettere foargongsrapportaazje, rângefaltsjes by delta-deteksje (`pipeline/lib/delta.py`).

**Dokuminten** — `docs/20260511/` (繁體中文; `00-*` is de yndeks). Oersettings fan de ûntwerp-dokuminten nei it Ingelsk binne wolkom.

### Basisregels

- Alle paden relatyf — it projekt moat oerdraachber wêze oer masines. Gjin fêstkodearre absolute paden.
- Foegje gjin pip-ôfhinklikens ta oan in *kearn*-module fan de pipeline. Opsjonele stadia meie opsjonele pakketten brûke, en moatte sierlik weromfalle sûnder harren.
- Ferswak de allinich-foarút state machine net — dat is it kostenplafond.
- Foegje gjin offisjele Amerikaanske oerheidsinsignia ta, en foegje neat ta dat boarne-redaksjes ûngedien makket.
- D1-skema-wizigings reitsje **twa** bestannen: `pipeline/lib/manifest_schema.sql` en `db/schema.sql`.
- Tests mei nije koade. Berjochten neffens Conventional-commit.

Lês earst `CLAUDE.md` en `docs/20260511/00-*`, en iepenje dan in issue om wat struktureels te besprekken foarôfgeand oan de PR.
