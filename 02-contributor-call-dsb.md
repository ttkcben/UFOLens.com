# GitHub — Póst 2 z 3 · Wołanje na sobustatkujucych / "dobre prědne nadawki"

**Wužyś:** ako pśipěta diskusija ("Sobustatkowanje & dobre prědne nadawki") abo zawjeźenje do CONTRIBUTING.md.
**Klucowe słowa:** open source, sobustatkowanje, dobry prědny nadawk, i18n, lokalizacija, OCR, Python, TypeScript, Vitest, pytest, pśistupnosć, UAP, wótwórjone daty
**Hyperwótkaze:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Sobustatkowanje na ufolens.com

[ufolens.com](https://www.ufolens.com) pśetwóri [archiw PURSUE UAP](https://www.war.gov/ufo) wójskego departamenta ZDA na pśepytujobnu, wěcejrěcnu platformu z [zjawnym API](https://www.ufolens.com/api/v1). Su to dwě połojcy — lokalny Python ingest pipeline (`pipeline/`) a TypeScript/Hono edge app (`worker/`) — kótarejž se na jadnom zwězowanju stakatej: wózjawjony SQL + asset-paket.

Njebrunicujśo žedne cloud-pówěrnosći, aby sobustatkowali. Jědrowe moduly pipeline'a su jano stdlib a testy Workerja se pśeśiwo in-memory-składowanju wuwjeźu.

### Nastajenje

```bash
# pipeline
python3 -m pytest pipeline/tests/          # by měło wšykno zelene byś, bźez pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Źož jo pomoc nejwužytnjejša

**i18n / lokalizacija** — `worker/src/i18n/ui-strings.json` jo žrědło za UI-teksty. Pśeglědanje kuždego njejendelskego lokalizowanja pśez maśeršćinarja jo wjelgin gódnotne: namakajśo njewšedne mašinowe wudaśa, pórědajśo RTL/layout-problemy, pólěpšajśo granicne pade pśi rěcnem dogronjenju.

**Kwalita OCR** — lěpše pśedpśeźěłowanje starych na pisańskej mašinje pisanych skenow pśed OCR; ewaluaciska mašina, kótaraž pśirownujo open-source engine z Tesseract-fallbackom na pśikładnych bokach.

**Pśistupnosć** — pśeglědajśo wótdane boki (`worker/src/render/`) pó WCAG; CSP jo strog (žeden `unsafe-inline`), togodla rozžognanja musy w tom ramiku funkcioněrowaś.

**API-ergonomija** — `worker/src/routes/` — paginacija, filtrowanje, OpenAPI-wopisanje, pśikładne klienty.

**Robustnosć pipeline'a** — wěcej drog za pśiměrje degraděrowanje, lěpše zdźělenje postupowanja, granicne pade pśi delta-detekciji (`pipeline/lib/delta.py`).

**Dokumentacija** — `docs/20260511/` (繁體中文; `00-*` jo indeks). Pśełožki designowych dokumentow do engelšćiny su witane.

### Zakładne pšawidła

- Wše sćažki relatiwne — projekt musy pśenosobny mjazy computerami byś. Žedne śěrde absolutne sćažki.
- Njedodawajśo pip-dependentnosć do *jědrowego* modula pipeline'a. Opcionalne schójźenki mógu opcionalne pakety wužywaś a musy pśiměrje bźez nich degraděrowaś.
- Njeslabiśo forward-only state machine — to jo kóńc kóstow.
- Njezwěsćujśo oficielne insignije kněžaŕstwa ZDA a njezadawajśo nic, což wótwrośijo žrědłowe redakcije.
- Změny D1-šemy se tykaju **dweju** datajowu: `pipeline/lib/manifest_schema.sql` a `db/schema.sql`.
- Testy z nowym kodom. Powěsći w stilu Conventional-commit.

Pśecytajśo nejpjerwjej `CLAUDE.md` a `docs/20260511/00-*`, potom wótwóriśo problem, aby diskutěrowali wó někajkich strukturelnych zmenach pśed PR.

