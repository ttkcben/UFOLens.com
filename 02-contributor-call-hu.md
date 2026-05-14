# GitHub — 2/3 bejegyzés · Hozzájárulói felhívás / "jó első feladatok"

**Felhasználás:** Rögzített Discussion-ként ("Hozzájárulás & jó első feladatok") vagy egy CONTRIBUTING.md bevezetőjeként.
**Kulcsszavak:** nyílt forráskód, hozzájárulás, jó első feladat, i18n, lokalizáció, OCR, Python, TypeScript, Vitest, pytest, akadálymentesítés, UAP, nyílt adatok
**Hiperhivatkozások:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Hozzájárulás az ufolens.com-hoz

Az [ufolens.com](https://www.ufolens.com) az Amerikai Egyesült Államok Hadügyminisztériumának [PURSUE UAP archívumát](https://www.war.gov/ufo) egy kereshető, többnyelvű platformmá alakítja egy [nyilvános API-val](https://www.ufolens.com/api/v1). Két részből áll — egy helyi Python adatfeldolgozó pipeline (`pipeline/`) és egy TypeScript/Hono edge alkalmazás (`worker/`) —, amelyek egyetlen interfészen találkoznak: egy közzétett SQL + eszközök csomagon.

Nincs szüksége felhő hozzáférésre a hozzájáruláshoz. A pipeline alapmoduljai csak a standard könyvtárat használják, a Worker tesztjei pedig memóriában tárolt adatokon futnak.

### Beállítás

```bash
# pipeline
python3 -m pytest pipeline/tests/          # mindennek zöldnek kell lennie, nincs szükség pip install-ra

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Hol a leghasznosabb a segítség

**i18n / lokalizáció** — a `worker/src/i18n/ui-strings.json` a felhasználói felület szövegeinek forrása. Bármely nem angol nyelvű lokalizáció anyanyelvi beszélő általi felülvizsgálata nagy értékű: a furcsa gépi fordítások kiszűrése, RTL/elrendezési problémák javítása, a nyelvi egyeztetés peremeseteinek javítása.

**OCR minőség** — a régi, írógéppel írt szkennelt oldalak jobb előfeldolgozása az OCR előtt; egy kiértékelő keretrendszer, amely összehasonlítja a nyílt forráskódú motort a Tesseract vészmegoldással mintalapokon.

**Akadálymentesítés** — a renderelt oldalak (`worker/src/render/`) auditálása a WCAG ellenében; a CSP szigorú (nincs `unsafe-inline`), így a megoldásoknak ezen belül kell működniük.

**API ergonómia** — `worker/src/routes/` — lapozás, szűrés, OpenAPI leírás, példa kliensek.

**Pipeline robusztusság** — több kecses degradációs útvonal, jobb folyamatjelentés, delta-detektálási peremesetek (`pipeline/lib/delta.py`).

**Dokumentáció** — `docs/20260511/` (繁體中文; a `00-*` az index). A tervezési dokumentumok angolra fordítását szívesen fogadjuk.

### Alapvető szabályok

- Minden elérési út relatív — a projektnek hordozhatónak kell lennie gépek között. Nincsenek mereven kódolt abszolút elérési utak.
- Ne adjon hozzá pip függőséget egy pipeline *alap* modulhoz. Az opcionális szakaszok használhatnak opcionális csomagokat, de kecsesen kell degradálódniuk nélkülük.
- Ne gyengítse a csak előre haladó állapotgépet — az a költségplafon.
- Ne vezessen be hivatalos USA kormányzati jelvényeket, és ne adjon hozzá semmit, ami visszafordítaná a forrás kitakarásait.
- A D1 séma módosításai **két** fájlt érintenek: `pipeline/lib/manifest_schema.sql` és `db/schema.sql`.
- Tesztek az új kóddal. Conventional Commits üzenetek.

Először olvassa el a `CLAUDE.md` és `docs/20260511/00-*` dokumentumokat, majd nyisson egy issue-t, hogy megvitassunk bármilyen strukturális változtatást a PR előtt.

