# GitHub — Postitus 2/3 · Kaastüütajidõ kutsõ / "hääq edimädseq ülesandõq"

**Kasuta nigu:** kinnütet arutelu ("Kaastüütamine ja hääq edimädseq ülesandõq") vai CONTRIBUTING.md sissejuhatus.
**Märksõnaq:** avalik lähtekood, kaastüütaminõ, hää edimäne ülesannõ, i18n, lokaliseeriminõ, OCR, Python, TypeScript, Vitest, pytest, ligipääsetävüs, UAP, avalik teedüs
**Hüperlingiq:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kaastüütaminõ ufolens.com-i jaos

[ufolens.com](https://www.ufolens.com) muudab USA Sõaministeeriumi [PURSUE UAP arhiivi](https://www.war.gov/ufo) otsitavas, mitmõkeelitses platvormis, millel om [avalik API](https://www.ufolens.com/api/v1). Tuu om kats poolt — paigapäälne Pythoni sisestüstorustik (`pipeline/`) ja TypeScript/Hono servarakendus (`worker/`) — miä kohtusõq üten liidesen: avaldõt SQL + varadõ komplekt.

Kaastüütämises olõ-iq vaja pilvetunnuseid. Torujuhtmõ tuumamooduliq ommaq õnnõ stdlib-põhised ja Workeri testiq tüütäseq mälusisese hoiusõ vasta.

### Sätestüs

```bash
# pipeline
python3 -m pytest pipeline/tests/          # piässi olõma kõik rohilidsõq, pip-i paigaldamist olõ-iq vaja

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kon abi om kõgõ inämb kasulik

**i18n / lokaliseeriminõ** — `worker/src/i18n/ui-strings.json` om kasutajaliidese tekstidõ lätt. Emakeelekõneleja ülevaatus mis taht mitte-inglisekeelse paikkunna kotsilõ om suurõ väärtusega: püüdäq kohmakat massintõlkimise tulõmust, parandaq RTL/paigutusprobleeme, parandaq keele läbirääkimise erijuhtumeid.

**OCR-i kvaliteet** — vanadõ kirätüümasinal kirotõt skaneeringidõ parõmb eeltüütlemine inne OCR-i; hindamisrakendus, miä võrdlõs avaligu lättetekstiga mootorit Tesseracti varuversiooniga näidislehtedel.

**Ligipääsetävüs** — auditeeriq renderdätüid lehti (`worker/src/render/`) WCAG-i vasta; CSP om karm (olõ-iq `unsafe-inline`), nii et lahendusõq piät tüütämä tuu sisen.

**API ergonoomika** — `worker/src/routes/` — lehekülgede jaotaminõ, filtreeriminõ, OpenAPI kirjeldüs, näidisklienti.

**Torujuhtmõ vastupidavus** — rohkõmb sujuva degradatsiooni radu, parõmb edenemisraport, delta-tuvastamisõ erijuhtumiq (`pipeline/lib/delta.py`).

**Dokumendiq** — `docs/20260511/` (繁體中文; `00-*` om register). Projekteerimisdokumentidõ tõlkõq inglise kiilde ommaq teretulnuq.

### Põhireegliq

- Kõik teeq ommaq suhtelised — projekt piät olõma masinate vahel kaasaskantav. Olõ-iq kõvastikoodit absoluutseid teid.
- Äräq lisaq pip-sõltuvust torujuhtmõ *tuum*moodulile. Valikulised astmõq võivaq pruukiq valikulisi pakette ja piät ilma nendeta sujuvalt degradeeruma.
- Äräq nõrgendaq õnnõ edespidist seisundimassiini — tuu om kulu ülemmäär.
- Äräq lisaq ammõtliidsi USA valitsusõ tunnismärke ja äräq lisaq midägi, miä pööraq tagasi lähtekoodi redigeerimisi.
- D1 skeemimuudatusõq pututasõq **katõ** faili: `pipeline/lib/manifest_schema.sql` ja `db/schema.sql`.
- Testiq vahtsõ koodiga. Konventsionaalsed commit-sõnumiq.

Lugege edimält `CLAUDE.md` ja `docs/20260511/00-*`, seejärel tehke probleem arutõlusõs inne PR-i mis taht struktuurse asja kotsilõ.

