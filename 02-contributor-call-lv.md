# GitHub — 2. no 3 ierakstiem · Aicinājums līdzstrādniekiem / "labi pirmie uzdevumi"

**Lietot kā:** piespraustu diskusiju ("Līdzdalība un labi pirmie uzdevumi") vai CONTRIBUTING.md ievadu.
**Atslēgvārdi:** atvērtais kods, līdzdalība, labs pirmais uzdevums, i18n, lokalizācija, OCR, Python, TypeScript, Vitest, pytest, pieejamība, UAP, atvērtie dati
**Hipersaites:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Līdzdalība ufolens.com projektā

[ufolens.com](https://www.ufolens.com) pārvērš ASV Kara departamenta [PURSUE UAP arhīvu](https://www.war.gov/ufo) par meklējamu, daudzvalodu platformu ar [publisku API](https://www.ufolens.com/api/v1). Tas sastāv no divām daļām — lokāla Python datu ievades konveijera (`pipeline/`) un TypeScript/Hono malu lietotnes (`worker/`) —, kas satiekas vienā saskarnē: publicētā SQL + resursu pakotnē.

Jums nav nepieciešami mākoņpakalpojumu akreditācijas dati, lai sniegtu savu ieguldījumu. Konveijera kodola moduļi ir tikai `stdlib`, un Worker testi darbojas pret atmiņā esošu krātuvi.

### Iestatīšana

```bash
# pipeline
python3 -m pytest pipeline/tests/          # visiem testiem jābūt sekmīgiem, nav nepieciešama pip instalācija

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kur palīdzība ir visnoderīgākā

**i18n / lokalizācija** — `worker/src/i18n/ui-strings.json` ir lietotāja saskarnes virkņu avots. Jebkuras ne-angļu valodas lokalizācijas pārskatīšana, ko veic dzimtās valodas runātājs, ir ļoti vērtīga: atrodiet neveiklu mašīntulkojumu, izlabojiet RTL/izkārtojuma problēmas, uzlabojiet valodu sarunu robežgadījumus.

**OCR kvalitāte** — labāka vecu, ar rakstāmmašīnu rakstītu skenējumu priekšapstrāde pirms OCR; novērtēšanas rīks, kas salīdzina atvērtā koda dzinēju ar Tesseract rezerves variantu uz parauglapām.

**Pieejamība** — auditējiet renderētās lapas (`worker/src/render/`) atbilstoši WCAG; CSP ir stingrs (nav `unsafe-inline`), tāpēc risinājumiem jādarbojas tā ietvaros.

**API ergonomika** — `worker/src/routes/` — lapošana, filtrēšana, OpenAPI apraksts, piemēru klienti.

**Konveijera robustums** — vairāk graciozas degradācijas ceļu, labāka progresa ziņošana, delta noteikšanas robežgadījumi (`pipeline/lib/delta.py`).

**Dokumentācija** — `docs/20260511/` (繁體中文; `00-*` ir indekss). Dizaina dokumentu tulkojumi angļu valodā ir laipni gaidīti.

### Pamatnoteikumi

- Visi ceļi ir relatīvi — projektam jābūt pārnēsājamam starp dažādām mašīnām. Nav cietkodētu absolūto ceļu.
- Nepievienojiet `pip` atkarību konveijera *kodola* modulim. Izvēles posmi var izmantot izvēles pakotnes, un tiem ir graciozi jādegradējas bez tām.
- Nevājiniet tikai uz priekšu vērsto stāvokļa mašīnu — tas ir izmaksu griestu nodrošinājums.
- Nepievienojiet oficiālas ASV valdības zīmotnes un neko, kas atceļ avota redakcijas.
- D1 shēmas izmaiņas skar **divus** failus: `pipeline/lib/manifest_schema.sql` un `db/schema.sql`.
- Testi kopā ar jauno kodu. Konvencionālo saistību ziņojumi (Conventional-commit messages).

Vispirms izlasiet `CLAUDE.md` un `docs/20260511/00-*`, pēc tam izveidojiet jaunu problēmas ziņojumu (issue), lai apspriestu jebkādas strukturālas izmaiņas pirms PR iesniegšanas.

