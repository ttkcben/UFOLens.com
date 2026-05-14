# GitHub – 2 iš 3 įrašų · Kvietimas prisidėti / „geros pirmosios užduotys“

**Naudoti kaip:** prisegtą diskusiją („Prisidėjimas ir geros pirmosios užduotys“) arba CONTRIBUTING.md įžangą.
**Raktažodžiai:** atvirasis kodas, prisidėjimas, gera pirmoji užduotis, i18n, lokalizavimas, OCR, Python, TypeScript, Vitest, pytest, prieinamumas, UAP, atviri duomenys
**Hipersaitai:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kaip prisidėti prie ufolens.com

[ufolens.com](https://www.ufolens.com) paverčia JAV Karo departamento [PURSUE UAP archyvą](https://www.war.gov/ufo) į paieškos galimybę turinčią, daugiakalbę platformą su [vieša API](https://www.ufolens.com/api/v1). Ją sudaro dvi dalys – vietinis Python duomenų įkėlimo dutotiekis (`pipeline/`) ir TypeScript/Hono edge programa (`worker/`) – susitinkančios vienoje sąsajoje: paskelbtame SQL + išteklių pakete.

Jums nereikia jokių debesijos prisijungimo duomenų, kad galėtumėte prisidėti. Dutotiekio branduolio moduliai yra tik stdlib, o Worker testai vykdomi su atmintyje esančia saugykla.

### Sąranka

```bash
# pipeline
python3 -m pytest pipeline/tests/          # turėtų būti viskas žalia, nereikia pip instaliuoti

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kur pagalba naudingiausia

**i18n / lokalizavimas** – `worker/src/i18n/ui-strings.json` yra vartotojo sąsajos eilučių šaltinis. Bet kurios ne anglų kalbos lokalės peržiūra, atlikta gimtakalbio, yra labai vertinga: pastebėti negrabius mašininio vertimo rezultatus, ištaisyti RTL/išdėstymo problemas, pagerinti kalbos derinimo kraštutinius atvejus.

**OCR kokybė** – geresnis senų, spausdintų mašinėle nuskaitytų dokumentų išankstinis apdorojimas prieš OCR; vertinimo sistema, lyginanti atvirojo kodo variklį su Tesseract atsarginiu variantu pavyzdiniuose puslapiuose.

**Prieinamumas** – atvaizduotų puslapių (`worker/src/render/`) auditas pagal WCAG; CSP yra griežtas (jokio `unsafe-inline`), todėl sprendimai turi veikti laikantis šio apribojimo.

**API ergonomika** – `worker/src/routes/` – puslapiavimas, filtravimas, OpenAPI aprašymas, pavyzdiniai klientai.

**Dutotiekio patikimumas** – daugiau sklandaus veikimo su apribojimais scenarijų, geresnis eigos ataskaitų teikimas, delta aptikimo kraštutiniai atvejai (`pipeline/lib/delta.py`).

**Dokumentai** – `docs/20260511/` (繁體中文; `00-*` yra indeksas). Projektavimo dokumentų vertimai į anglų kalbą yra laukiami.

### Pagrindinės taisyklės

- Visi keliai yra santykiniai – projektas turi būti nešiojamas tarp skirtingų kompiuterių. Jokių griežtai nurodytų absoliučių kelių.
- Nepridėkite pip priklausomybės prie dutotiekio *branduolio* modulio. Pasirenkami etapai gali naudoti pasirenkamus paketus ir turi sklandžiai veikti su apribojimais be jų.
- Nesilpninkite „tik pirmyn“ būsenų mašinos – tai yra sąnaudų lubos.
- Nepridėkite oficialios JAV vyriausybės simbolikos ir nieko, kas atšauktų pirminių šaltinių redagavimus.
- D1 schemos pakeitimai liečia **du** failus: `pipeline/lib/manifest_schema.sql` ir `db/schema.sql`.
- Testai su nauju kodu. Conventional-commit pranešimai.

Pirmiausia perskaitykite `CLAUDE.md` ir `docs/20260511/00-*`, tada atidarykite klaidų pranešimą, kad aptartumėte bet kokius struktūrinius pakeitimus prieš teikdami PR.

