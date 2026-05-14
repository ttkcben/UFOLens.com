# GitHub — Objava 2 od 3 · Poziv za suradnike / "dobri prvi zadaci"

**Koristiti kao:** zakačenu diskusiju ("Doprinos & dobri prvi zadaci") ili uvod u CONTRIBUTING.md.
**Ključne riječi:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hiperveze:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Doprinos projektu ufolens.com

[ufolens.com](https://www.ufolens.com) pretvara [PURSUE UAP arhiv](https://www.war.gov/ufo) Ministarstva rata SAD-a u pretraživu, višejezičnu platformu s [javnim API-jem](https://www.ufolens.com/api/v1). Sastoji se od dvije polovine — lokalnog Python pipelinea za unos podataka (`pipeline/`) i TypeScript/Hono edge aplikacije (`worker/`) — koje se susreću na jednom sučelju: objavljenom SQL + assets paketu.

Za doprinos vam nisu potrebne nikakve cloud vjerodajnice. Jezgreni moduli pipelinea koriste samo stdlib, a testovi za Worker se izvršavaju na memorijskom pohranjivanju.

### Postavljanje

```bash
# pipeline
python3 -m pytest pipeline/tests/          # trebalo bi sve biti zeleno, nije potrebna pip instalacija

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Gdje je pomoć najkorisnija

**i18n / lokalizacija** — `worker/src/i18n/ui-strings.json` je izvor UI stringova. Pregled od strane izvornog govornika bilo kojeg ne-engleskog lokaliteta je od velike vrijednosti: uočavanje nezgrapnog mašinskog prijevoda, ispravljanje RTL/layout problema, poboljšanje rubnih slučajeva pregovaranja jezika.

**Kvaliteta OCR-a** — bolja pred-obrada starih, pisaćom mašinom pisanih skenova prije OCR-a; evaluacijski set za usporedbu open-source enginea s Tesseract rezervnom opcijom na uzorcima stranica.

**Pristupačnost** — provjera renderiranih stranica (`worker/src/render/`) prema WCAG standardima; CSP je strog (nema `unsafe-inline`), tako da rješenja moraju raditi unutar toga.

**Ergonomija API-ja** — `worker/src/routes/` — paginacija, filtriranje, OpenAPI opis, primjeri klijenata.

**Robusnost pipelinea** — više puteva za gracioznu degradaciju, bolje izvještavanje o napretku, rubni slučajevi detekcije delte (`pipeline/lib/delta.py`).

**Dokumentacija** — `docs/20260511/` (繁體中文; `00-*` je indeks). Prijevodi projektne dokumentacije na engleski su dobrodošli.

### Osnovna pravila

- Sve staze su relativne — projekt mora biti prenosiv između mašina. Nema fiksno kodiranih apsolutnih staza.
- Nemojte dodavati pip ovisnost u *jezgreni* modul pipelinea. Opcionalne faze mogu koristiti opcionalne pakete, i moraju se graciozno degradirati bez njih.
- Nemojte slabiti mašinu stanja "samo naprijed" — to je gornja granica troškova.
- Nemojte uvoditi službene oznake vlade SAD-a, i ne dodajite ništa što poništava redakcije izvora.
- Promjene D1 sheme dotiču **dva** fajla: `pipeline/lib/manifest_schema.sql` i `db/schema.sql`.
- Testovi s novim kodom. Poruke commitova prema Conventional Commits standardu.

Prvo pročitajte `CLAUDE.md` i `docs/20260511/00-*`, a zatim otvorite prijavu problema kako biste razgovarali o bilo kakvim strukturalnim promjenama prije PR-a.
