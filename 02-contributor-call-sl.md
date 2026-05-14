# GitHub — Objave 2 od 3 · Poziv k sodelovanju / "dobre prve naloge"

**Uporaba:** kot pripeta razprava ("Prispevanje in dobre prve naloge") ali uvod v CONTRIBUTING.md.
**Ključne besede:** odprta koda, prispevanje, dobra prva naloga, i18n, lokalizacija, OCR, Python, TypeScript, Vitest, pytest, dostopnost, UAP, odprti podatki
**Hiperpovezave:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Prispevanje k ufolens.com

[ufolens.com](https://www.ufolens.com) spreminja [arhiv PURSUE UAP](https://www.war.gov/ufo) Ministrstva za obrambo ZDA v iskalno, večjezično platformo z [javnim API-jem](https://www.ufolens.com/api/v1). Sestavljen je iz dveh polovic — lokalnega cevovoda za vnos v Pythonu (`pipeline/`) in robne aplikacije v TypeScript/Hono (`worker/`) — ki se srečata na enem vmesniku: objavljenem svežnju SQL + sredstev.

Za prispevanje ne potrebujete nobenih poverilnic za oblak. Jedrni moduli cevovoda so samo s stdlib, testi za Worker pa se izvajajo proti pomnilniku v spominu.

### Namestitev

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kjer je pomoč najbolj koristna

**i18n / lokalizacija** — `worker/src/i18n/ui-strings.json` je vir nizov za uporabniški vmesnik. Pregled katere koli neangleške lokalizacije s strani naravnega govorca je zelo dragocen: odkrijte nerodne strojne prevode, popravite težave z RTL/postavitvijo, izboljšajte robne primere pri pogajanju o jeziku.

**Kakovost OCR** — boljša predobdelava starih tipkanih skenov pred OCR; ocenjevalni sistem za primerjavo odprtokodnega pogona z rezervnim Tesseract na vzorčnih straneh.

**Dostopnost** — preverite upodobljene strani (`worker/src/render/`) glede na WCAG; CSP je strog (brez `unsafe-inline`), zato morajo rešitve delovati znotraj tega.

**Ergonomija API-ja** — `worker/src/routes/` — paginacija, filtriranje, opis OpenAPI, primeri odjemalcev.

**Robustnost cevovoda** — več poti za elegantno zmanjšanje, boljše poročanje o napredku, robni primeri pri odkrivanju razlik (`pipeline/lib/delta.py`).

**Dokumentacija** — `docs/20260511/` (繁體中文; `00-*` je kazalo). Prevodi oblikovalske dokumentacije v angleščino so dobrodošli.

### Osnovna pravila

- Vse poti so relativne — projekt mora biti prenosljiv med računalniki. Brez trdo kodiranih absolutnih poti.
- Ne dodajajte odvisnosti pip v *jedrni* modul cevovoda. Izbirne faze lahko uporabljajo izbirne pakete in se morajo elegantno zmanjšati brez njih.
- Ne slabite stanja stroja samo naprej — to je zgornja meja stroškov.
- Ne dodajajte uradnih oznak vlade ZDA in ničesar, kar bi razveljavilo redakcije v viru.
- Spremembe sheme D1 se dotikajo **dveh** datotek: `pipeline/lib/manifest_schema.sql` in `db/schema.sql`.
- Testi z novo kodo. Sporočila o oddaji v slogu Conventional-commit.

Najprej preberite `CLAUDE.md` in `docs/20260511/00-*`, nato odprite prijavo napake za razpravo o čemerkoli strukturnem pred PR.

