# GitHub – Příspěvek 2 ze 3 · Výzva pro přispěvatele / „dobré první úkoly“

**Použití jako:** připnutá diskuze („Přispívání a dobré první úkoly“) nebo úvod do `CONTRIBUTING.md`.
**Klíčová slova:** open source, přispívání, dobrý první úkol, i18n, lokalizace, OCR, Python, TypeScript, Vitest, pytest, přístupnost, UAP, otevřená data
**Hypertextové odkazy:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Přispívání do ufolens.com

[ufolens.com](https://www.ufolens.com) přeměňuje [archiv PURSUE UAP](https://www.war.gov/ufo) amerického Ministerstva války na prohledávatelnou, vícejazyčnou platformu s [veřejným API](https://www.ufolens.com/api/v1). Skládá se ze dvou polovin – lokální Python ingestovací pipeline (`pipeline/`) a edge aplikace v TypeScript/Hono (`worker/`) – které se setkávají na jednom rozhraní: publikovaném balíčku SQL + aktiv.

K přispívání nepotřebujete žádné cloudové přihlašovací údaje. Jádrové moduly pipeline jsou závislé pouze na standardní knihovně (stdlib-only) a testy Workeru běží proti paměťovému úložišti.

### Nastavení

```bash
# pipeline
python3 -m pytest pipeline/tests/          # vše by mělo projít, není potřeba instalace přes pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kde je pomoc nejužitečnější

**i18n / lokalizace** – `worker/src/i18n/ui-strings.json` je zdrojovým souborem pro řetězce uživatelského rozhraní. Recenze od rodilého mluvčího jakéhokoli neanglického jazyka má vysokou hodnotu: odhalení neobratného strojového překladu, oprava problémů s RTL/rozložením, vylepšení okrajových případů vyjednávání jazyka.

**Kvalita OCR** – lepší předzpracování starých, na stroji psaných skenů před OCR; testovací nástroj porovnávající open-source engine s Tesseract zálohou na vzorových stránkách.

**Přístupnost** – audit renderovaných stránek (`worker/src/render/`) oproti WCAG; CSP je striktní (žádné `unsafe-inline`), takže řešení musí fungovat v těchto mezích.

**Ergonomie API** – `worker/src/routes/` – stránkování, filtrování, OpenAPI popis, příklady klientů.

**Robustnost pipeline** – více cest pro elegantní degradaci, lepší hlášení postupu, okrajové případy detekce delty (`pipeline/lib/delta.py`).

**Dokumentace** – `docs/20260511/` (繁體中文; `00-*` je index). Překlady návrhové dokumentace do angličtiny jsou vítány.

### Základní pravidla

- Všechny cesty jsou relativní – projekt musí být přenositelný mezi stroji. Žádné natvrdo zakódované absolutní cesty.
- Nepřidávejte závislost přes pip do *jádrového* modulu pipeline. Volitelné fáze mohou používat volitelné balíčky a musí elegantně degradovat bez nich.
- Neoslabujte stavový automat pouze pro posun vpřed – to je strop nákladů.
- Nepřidávejte oficiální insignie vlády USA a nepřidávejte nic, co by odstraňovalo zdrojové redakční úpravy.
- Změny schématu D1 se dotýkají **dvou** souborů: `pipeline/lib/manifest_schema.sql` a `db/schema.sql`.
- Nový kód musí mít testy. Zprávy ke commitům musí dodržovat Conventional Commits.

Nejprve si přečtěte `CLAUDE.md` a `docs/20260511/00-*`, poté otevřete issue k prodiskutování jakýchkoli strukturálních změn před vytvořením PR.

