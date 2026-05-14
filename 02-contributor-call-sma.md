# GitHub — Båatsoe 2 3:este · Meedebarkoe-baakoe / "buere voestes issues"

**Nuhtjh goh:** vïedtjestamme Diskusjovne ("Meedebarkeminie & buere voestes issues") jallh CONTRIBUTING.md intro.
**Sleatkoe-baakoeh:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyper-laangh:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Meedebarkeminie ufolens.com'ese

[ufolens.com](https://www.ufolens.com) jeatjahte U.S. Soete-departementen [PURSUE UAP-arkijvem](https://www.war.gov/ufo) ohtsemes, gellie-gïeleldh byjjeskuevtese [almetjijhke API'ine](https://www.ufolens.com/api/v1). Dïhte göökte bielieh — voenges Python ingest-pipeline (`pipeline/`) jïh TypeScript/Hono kant-app (`worker/`) — gaavnedeminie akte interface'isnie: publiseereme SQL + assets bundle.

Ih daarpesjh gïemhke pilve-kredensjaalh meedebarkeminie. Pipeline'n jarnge-modulh leah stdlib-only jïh Worker-testh leah jåhteme in-memory laahkege.

### Sjïehtesjimmie

```bash
# pipeline
python3 -m pytest pipeline/tests/          # byøroe gaajhkh vïenhtes, ij pip install daarpesjh

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Gusnie viehkie lea sïejhmesommes

**i18n / voengesjimmie** — `worker/src/i18n/ui-strings.json` lea valke UI-teekstide. Etniske-gïeleldh vaajmoe-gïehtjedalleme maam gïeleldh ij engelske lea jolle-aerveste: gïehtjede geerve maasjine-jarkoestimmieh, sjïehtesjidh RTL/layout-problemh, bueriedidh gïele-negotiasjons-kant-tilfelleh.

**OCR-kvaliteete** — buerebe åvteli-gïehtjedalleme gåmmel typewritten scannijste åvteli OCR; evaluerings-harness gïeh compareminie reahkes-valke mootore Tesseract-fallback'en vööste sample-sæjrojne.

**Tjïelkesvoete** — auditerh rendereme sæjrojh (`worker/src/render/`) WCAG'en vööste; CSP lea gïerve (ij `unsafe-inline`), dellie sjïehtesjimmieh byøroeh barkoedh daan sisnie.

**API-ergonomije** — `worker/src/routes/` — pagination, filtering, OpenAPI-deskripsjovne, sample-klijenth.

**Pipeline-kreavsoe** — vielie gracefult-degradere-geajnh, buerebe progress-reportinge, delta-deteksjons-kant-tilfelleh (`pipeline/lib/delta.py`).

**Dokumeenth** — `docs/20260511/` (繁體中文; `00-*` lea indekse). Jarkoestimmieh design-dokumeentijste engelskese leah buere-baateme.

### Vejkieve-reejkelh

- Gaajhkh geajnh relative — prosjekte byøroe fealadidh maehtedh maasjine-gaskes. Ij gïemhke gïerve absolutt geajnh.
- Aellieh lissiehtidh pip-lahtestimmieh pipeline *jarnge*-module. Opsjonelle staaleh maehtieh opsjonelle pakkerh nuhtjedh, jïh byøroeh gracefult degradere bielelen dej.
- Aellieh haemkiehtidh voeride-guvvie staatemaasjinem — dïhte lea kåaste-lååpome.
- Aellieh voeremhke U.S. reeremen mïerkh lissiehtidh, jïh aellieh maam lissiehtidh gïeh valke-redaksjovnh baalhkh.
- D1-skema-jeatjemh leah göökte-filh gïehtjedalleme: `pipeline/lib/manifest_schema.sql` jïh `db/schema.sql`.
- Testh orre koodine. Dåabislig-commit-båaatsoeh.

Maaje `CLAUDE.md` jïh `docs/20260511/00-*` voeste, dellie ååple issue'm åvteste diskuterh maam structural åvteli PR.

