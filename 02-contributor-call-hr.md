# GitHub — Objava 2 od 3 · Poziv za suradnike / "dobri prvi zadaci"

**Koristiti kao:** prikvačenu raspravu ("Doprinos i dobri prvi zadaci") ili uvod u CONTRIBUTING.md.
**Ključne riječi:** open source, doprinos, dobar prvi zadatak, i18n, lokalizacija, OCR, Python, TypeScript, Vitest, pytest, pristupačnost, UAP, otvoreni podaci
**Poveznice:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Doprinos projektu ufolens.com

[ufolens.com](https://www.ufolens.com) pretvara arhivu [PURSUE UAP](https://www.war.gov/ufo) američkog Ministarstva rata u pretraživu, višejezičnu platformu s [javnim API-jem](https://www.ufolens.com/api/v1). Sastoji se od dvije polovice — lokalnog Python cjevovoda za unos (`pipeline/`) i TypeScript/Hono rubne aplikacije (`worker/`) — koje se susreću na jednom sučelju: objavljenom paketu SQL + resursi.

Za doprinos vam nisu potrebne nikakve vjerodajnice za oblak. Jezgreni moduli cjevovoda koriste samo stdlib, a testovi za Worker se izvode protiv pohrane u memoriji.

### Postavljanje

```bash
# cjevovod
python3 -m pytest pipeline/tests/          # sve bi trebalo biti zeleno, nije potrebna pip instalacija

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Gdje je pomoć najkorisnija

**i18n / lokalizacija** — `worker/src/i18n/ui-strings.json` je izvor UI nizova. Pregled bilo kojeg ne-engleskog jezika od strane izvornog govornika je od velike vrijednosti: uočavanje nezgrapnih strojnih prijevoda, popravljanje problema s RTL/rasporedom, poboljšanje rubnih slučajeva jezičnog pregovaranja.

**Kvaliteta OCR-a** — bolja pred-obrada starih tipkanih skenova prije OCR-a; sustav za evaluaciju koji uspoređuje open-source mehanizam s Tesseract rezervom na uzorcima stranica.

**Pristupačnost** — provjera renderiranih stranica (`worker/src/render/`) prema WCAG; CSP je strog (bez `unsafe-inline`), pa rješenja moraju raditi unutar toga.

**Ergonomija API-ja** — `worker/src/routes/` — paginacija, filtriranje, OpenAPI opis, primjeri klijenata.

**Robusnost cjevovoda** — više puteva za gracioznu degradaciju, bolje izvještavanje o napretku, rubni slučajevi detekcije delte (`pipeline/lib/delta.py`).

**Dokumentacija** — `docs/20260511/` (繁體中文; `00-*` je indeks). Prijevodi projektne dokumentacije na engleski su dobrodošli.

### Osnovna pravila

- Sve staze su relativne — projekt mora biti prenosiv na različitim strojevima. Nema tvrdo kodiranih apsolutnih staza.
- Nemojte dodavati pip ovisnost u *jezgreni* modul cjevovoda. Opcionalne faze mogu koristiti opcionalne pakete i moraju graciozno degradirati bez njih.
- Nemojte slabiti stroj stanja koji ide samo naprijed — to je gornja granica troškova.
- Nemojte uvoditi službene oznake vlade SAD-a i nemojte dodavati ništa što poništava redakcije izvora.
- Promjene D1 sheme dotiču **dvije** datoteke: `pipeline/lib/manifest_schema.sql` i `db/schema.sql`.
- Testovi s novim kodom. Poruke o izvršenju prema Conventional Commits.

Prvo pročitajte `CLAUDE.md` i `docs/20260511/00-*`, a zatim otvorite problem kako biste raspravili o bilo čemu strukturnom prije PR-a.

