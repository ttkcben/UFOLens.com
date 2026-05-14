# GitHub — Indatshana 2 kwezi-3 · Isimemo sabanikeli / "iimiba emihle yokuthoma"

**Sebenzisa njenge-:** Ingxoxo ebethelwe ("Ukunikela & iimiba emihle yokuthoma") namkha isingeniso se-CONTRIBUTING.md.
**Amagamaqangi:** i-open source, ukunikela, umba omuhle wokuthoma, i-i18n, ukwenza kwanqhema, i-OCR, i-Python, i-TypeScript, i-Vitest, i-pytest, ukufinyeleleka, i-UAP, idatha evulekileko
**Izixhumanisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ukunikela ku-ufolens.com

[ufolens.com](https://www.ufolens.com) itjhugulula i-[PURSUE UAP archive](https://www.war.gov/ufo) yomNyango weZemphi wase-U.S. ibe sibaya esingahlolisiseka, esineelimi ezinengi esine-[API yomphakathi](https://www.ufolens.com/api/v1). Zizigaba ezimbili — i-pipeline ye-Python yangekhaya (`pipeline/`) kunye ne-TypeScript/Hono edge app (`worker/`) — ezihlangana esikhungweni esisodwa: i-SQL esiveziweko + i-assets bundle.

Awudingi zikghona ze-cloud ukunikela. Ama-module we-core we-pipeline yi-stdlib-only begodu ama-test we-Worker asebenza ngokugcina nge-memory.

### Ukubeka

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Lapho usizo lusebenziseka khona khulu

**i18n / ukwenza kwanqhema** — `worker/src/i18n/ui-strings.json` ngumthombo weentambo ze-UI. Ukuhlola komkhulumi womdabu kwelimi elingasilo isiNgisi kunenani eliphezulu: thola okukhiqizwa komshini okungakhululekiko, lungisa iimiba ze-RTL/ukubeka, thuthukisa iindawo zokukhulumisana ngeelimi.

**Ikhwalithi ye-OCR** — ukulungiswa okungcono kweemifanekiso ezindala ezilotjhiwe nge-typewriter ngaphambi kwe-OCR; isitjhudulu sokuhlola esiqathanisa injini ye-open-source ne-Tesseract fallback emakhasini wesampula.

**Ukufinyeleleka** — hlola amakhasi aveziweko (`worker/src/render/`) ngokuvumelana ne-WCAG; i-CSP iqinile (akukho `unsafe-inline`), ngalokho iimhlahlandlela kufanele zisebenze ngaphakathi kwalokho.

**I-API ergonomics** — `worker/src/routes/` — ukubeka amakhasi, ukuhlola, incazelo ye-OpenAPI, ama-client wesampula.

**Ukuqina kwe-Pipeline** — iindlela ezinengi zokuncipha ngobunono, ukubika okungcono kwentuthuko, iindawo zokuthola i-delta (`pipeline/lib/delta.py`).

**Amaphepha** — `docs/20260511/` (繁體中文; `00-*` yi-index). Ukuhumusha kwamaphepha wokuklama esiNgisini kwamukelekile.

### Imithetho eyisisekelo

- Zoke iindlela zihlobene — iprojekthi kufanele ikghone ukusebenza kiimishini ezinengi. Akukho iindlela ezipheleleko ezibhalwe khona.
- Ungafaki i-pip dependency ku-pipeline *core* module. Izigaba zokuzikhethela zingasebenzisa ama-package wokuzikhethela, begodu kufanele zinciphe ngobunono ngaphandle kwawo.
- Ungawubutshelisi umshini we-state oya phambili kwaphela — loyo ngumkhawulo wendleko.
- Ungafaki iimbotjhisi ezisemthethweni zikarhulumende we-U.S., begodu ungafaki khabe okungenqabula ukungenelela komthombo.
- Iintjhijilelo ze-D1 schema zithinta amafayela **amabili**: `pipeline/lib/manifest_schema.sql` kunye ne-`db/schema.sql`.
- Ama-test anekhowudi elitjha. Imilayezo ye-Conventional-commit.

Funda i-`CLAUDE.md` kunye ne-`docs/20260511/00-*` qangi, bese uvula umba wokukhuluma ngomahlongandlebe ngaphambi kwe-PR.

