# GitHub — Přispawk 2 z 3 · Wołanje k přinošowanju / "dobre prěnje nadawki"

**Wužij jako:** připjaty slěd diskusije ("Přinošowanje & dobre prěnje nadawki") abo wuwod do CONTRIBUTING.md.
**Klučowe hesła:** open source, přinošowanje, dobry prěni nadawk, i18n, lokalizacija, OCR, Python, TypeScript, Vitest, pytest, přistupnosć, UAP, wotewrjene daty
**Wotkazy:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Přinošowanje k ufolens.com

[ufolens.com](https://www.ufolens.com) přetworja [PURSUE UAP-archiw](https://www.war.gov/ufo) wójnskeho departmenta ZSA do přepytać dacaceje so wjacerěčneje platformy z [zjawnym API](https://www.ufolens.com/api/v1). Wono wobsteji z dweju połojcow — lokalneho Python ingest-pipeline (`pipeline/`) a TypeScript/Hono edge-aplikacije (`worker/`) — kotrejž so na jednym interfejsu zetkatej: wozjewjenym SQL + zasobowym zwjazku.

Njetrjebaće žane mróčelowe přizjewjenske daty, zo byšće přinošowali. Jadrowe moduly pipeline su jenož ze standardnej biblioteku (stdlib-only) a testy Workera běža přećiwo in-memory-składowanju.

### Nastajenje

```bash
# pipeline
python3 -m pytest pipeline/tests/          # měło by wšo zelene być, pip-instalacija njeje trěbna

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Hdźe je pomoc najwužitniša

**i18n / lokalizacija** — `worker/src/i18n/ui-strings.json` je žórło za tekstowe elementy na wužiwarskim powjerchu. Posudźenje wot maćernorěčnika za kóždu njerozrjadowanu lokalizaciju je jara hódnotne: namakać njewobratne mašinowe wudaće, porjedźić RTL/layout-problemy, polěpšić mjeńše pady při rěčnym jednani.

**Kwalita OCR** — lěpše předpředźěłanje starych na pisanym stroju pisanych skenowanjow před OCR; evalwaciski grat, kotryž přirunuje open-source engine z Tesseract-fallback na přikładowych stronach.

**Přistupnosć** — přepruwować wuhotowane strony (`worker/src/render/`) přećiwo WCAG; CSP je kruty (nic `unsafe-inline`), tohodla dyrbja rozrisanja znutřka toho fungować.

**API-ergonomija** — `worker/src/routes/` — paginacija, filtrowanje, OpenAPI-wopis, přikładowi klijenća.

**Robustnosć pipeline** — wjace pućow za zmilniwe degradowanje, lěpše zdźělenje postupow, mjeńše pady při delta-detekciji (`pipeline/lib/delta.py`).

**Dokumentacija** — `docs/20260511/` (繁體中文; `00-*` je indeks). Přełožki designowych dokumentow do jendźelšćiny su witane.

### Zakładne prawidła

- Wšě puće relatiwne — projekt dyrbi přenošujomny mjez mašinami być. Žane krute absolutne puće.
- Njepřidawajće pip-wotwisnosć k *jadrowemu* modulej pipeline. Opcionalne schodźenki móža opcionalne pakćiki wužiwać a dyrbja bjez nich zmilniwje degradować.
- Njesłabńće mašinu z stawom, kotraž dźěła jenož doprědka — to je kóštowy strop.
- Njezzwěsćujće oficielne insignije knježerstwa ZSA a njepřidawajće ničo, štož by žórłowe redakcije wróćo wobroćiło.
- Změny šemy D1 dótknu **dwě** dataji: `pipeline/lib/manifest_schema.sql` a `db/schema.sql`.
- Testy z nowym kodom. Powěsće z konwencionalnymi commit-zdźělenkami.

Přečitajće najprjedy `CLAUDE.md` a `docs/20260511/00-*`, potom wočińće wjelkostatk, zo byšće wo strukturelnych zaležnosćach před PR diskutowali.

