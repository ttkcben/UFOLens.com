# GitHub – Príspevok 2 z 3 · Výzva pre prispievateľov / "dobré prvé úlohy"

**Použitie ako:** pripnutá diskusia ("Prispievanie a dobré prvé úlohy") alebo úvod do CONTRIBUTING.md.
**Kľúčové slová:** open source, prispievanie, dobrá prvá úloha, i18n, lokalizácia, OCR, Python, TypeScript, Vitest, pytest, prístupnosť, UAP, otvorené dáta
**Hypertextové odkazy:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Prispievanie do ufolens.com

[ufolens.com](https://www.ufolens.com) mení archív [PURSUE UAP](https://www.war.gov/ufo) od Ministerstva vojny USA na prehľadávateľnú, viacjazyčnú platformu s [verejným API](https://www.ufolens.com/api/v1). Skladá sa z dvoch častí — lokálneho Python pipeline na spracovanie dát (`pipeline/`) a edge aplikácie v TypeScript/Hono (`worker/`) — ktoré sa stretávajú v jednom rozhraní: publikovanom balíku SQL + assetov.

Na prispievanie nepotrebujete žiadne cloudové prihlasovacie údaje. Jadrové moduly pipeline sú závislé len od stdlib a testy pre Worker bežia proti in-memory úložisku.

### Nastavenie

```bash
# pipeline
python3 -m pytest pipeline/tests/          # všetko by malo prejsť, nie je potrebný žiadny pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kde je pomoc najužitočnejšia

**i18n / lokalizácia** — `worker/src/i18n/ui-strings.json` je zdrojom reťazcov pre UI. Kontrola rodeným hovorcom akejkoľvek inej ako anglickej lokalizácie je veľmi cenná: odhaliť neobratný strojový preklad, opraviť problémy s RTL/rozložením, vylepšiť okrajové prípady pri dohadovaní jazyka.

**Kvalita OCR** — lepšie predspracovanie starých, písacím strojom písaných skenov pred OCR; hodnotiaci nástroj porovnávajúci open-source engine so záložným Tesseractom na vzorových stranách.

**Prístupnosť** — audit renderovaných stránok (`worker/src/render/`) podľa WCAG; CSP je prísne (žiadne `unsafe-inline`), takže riešenia musia fungovať v rámci tohto obmedzenia.

**Ergonómia API** — `worker/src/routes/` — stránkovanie, filtrovanie, OpenAPI popis, príklady klientov.

**Robustnosť pipeline** — viac ciest pre elegantnú degradáciu, lepšie reportovanie priebehu, okrajové prípady detekcie delta (`pipeline/lib/delta.py`).

**Dokumentácia** — `docs/20260511/` (繁體中文; `00-*` je index). Preklady návrhovej dokumentácie do angličtiny sú vítané.

### Základné pravidlá

- Všetky cesty sú relatívne — projekt musí byť prenosný medzi strojmi. Žiadne napevno zakódované absolútne cesty.
- Nepridávajte pip závislosť do *jadra* pipeline modulu. Voliteľné fázy môžu používať voliteľné balíčky a musia sa elegantne degradovať bez nich.
- Neoslabujte jednosmerný stavový automat — to je strop nákladov.
- Nepridávajte oficiálne insígnie vlády USA a nič, čo by odstraňovalo začiernené texty zo zdrojov.
- Zmeny D1 schémy sa dotýkajú **dvoch** súborov: `pipeline/lib/manifest_schema.sql` a `db/schema.sql`.
- Testy s novým kódom. Správy commitov podľa konvencie (Conventional Commits).

Najprv si prečítajte `CLAUDE.md` a `docs/20260511/00-*`, potom otvorte issue, aby sme prediskutovali akékoľvek štrukturálne zmeny pred vytvorením PR.

