# GitHub — Postitus 2/3 · Kaastööliste kutse / "head esimesed ülesanded"

**Kasutus:** kinnitatud aruteluna ("Kaasaaitamine ja head esimesed ülesanded") või CONTRIBUTING.md sissejuhatusena.
**Märksõnad:** avatud lähtekood, kaasaaitamine, hea esimene ülesanne, i18n, lokaliseerimine, OCR, Python, TypeScript, Vitest, pytest, ligipääsetavus, UAP, avaandmed
**Hüperlingid:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Panustamine ufolens.com-i arendusse

[ufolens.com](https://www.ufolens.com) muudab USA sõjaministeeriumi [PURSUE UAP arhiivi](https://www.war.gov/ufo) otsitavaks, mitmekeelseks platvormiks koos [avaliku API-ga](https://www.ufolens.com/api/v1). See koosneb kahest poolest — lokaalne Pythoni sisestustoru (`pipeline/`) ja TypeScript/Hono servarakendus (`worker/`) —, mis kohtuvad ühes liideses: avaldatud SQL + varade kimp.

Panustamiseks ei vaja te pilvevolitusi. Toru põhimoodulid on ainult stdlib-põhised ja Workeri testid töötavad mälusisese salvestuse vastu.

### Seadistus

```bash
# pipeline
python3 -m pytest pipeline/tests/          # kõik peaks olema roheline, pip installi pole vaja

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kus on abi kõige kasulikum

**i18n / lokaliseerimine** — `worker/src/i18n/ui-strings.json` on kasutajaliidese stringide allikas. Iga mitte-ingliskeelse lokaadi emakeelekõneleja poolt tehtud ülevaatus on väga väärtuslik: kohmakate masintõlke väljundite parandamine, RTL/paigutusprobleemide lahendamine, keeleläbirääkimiste erijuhtumite täiustamine.

**OCR kvaliteet** — vanade masinakirjas skaneeringute parem eeltöötlus enne OCR-i; hindamisrakis, mis võrdleb avatud lähtekoodiga mootorit Tesseracti varuvariandiga näidislehtedel.

**Ligipääsetavus** — renderdatud lehtede (`worker/src/render/`) auditeerimine WCAG vastu; CSP on range (ei ole `unsafe-inline`), seega peavad lahendused selle raames töötama.

**API ergonoomika** — `worker/src/routes/` — lehekülgede kaupa jaotamine, filtreerimine, OpenAPI kirjeldus, näidisklientrakendused.

**Toru vastupidavus** — rohkem sujuva taandumise teid, parem edenemise aruandlus, delta-tuvastuse erijuhud (`pipeline/lib/delta.py`).

**Dokumentatsioon** — `docs/20260511/` (繁體中文; `00-*` on register). Disainidokumentide tõlked inglise keelde on teretulnud.

### Põhireeglid

- Kõik teed on suhtelised — projekt peab olema kaasaskantav erinevate masinate vahel. Ei mingeid koodi sisse kirjutatud absoluutseid teid.
- Ärge lisage pip-sõltuvust toru *põhi*moodulile. Valikulised etapid võivad kasutada valikulisi pakette ja peavad ilma nendeta sujuvalt taanduma.
- Ärge nõrgestage ainult edasiliikuvat olekumasinat — see on kululagi.
- Ärge lisage ametlikke USA valitsuse sümboleid ja ärge lisage midagi, mis tühistab lähteallika redigeerimisi.
- D1 skeemi muudatused puudutavad **kahte** faili: `pipeline/lib/manifest_schema.sql` ja `db/schema.sql`.
- Uue koodiga kaasnevad testid. Conventional-commit sõnumid.

Lugege enne PR-i tegemist läbi `CLAUDE.md` ja `docs/20260511/00-*`, seejärel avage probleem, et arutada midagi struktuurset.

