# GitHub — 2/3 Argitalpena · Kolaboratzaileentzako deia / "lehen issue onak"

**Erabiltzeko modua:** ainguratutako Discussion bat ("Contributing & good first issues") edo CONTRIBUTING.md sarrera bat.
**Gako-hitzak:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hiperestekak:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com-en laguntzen

[ufolens.com](https://www.ufolens.com)-ek AEBetako Gerra Sailaren [PURSUE UAP artxiboa](https://www.war.gov/ufo) plataforma bilagarri eta eleaniztun bihurtzen du [API publiko](https://www.ufolens.com/api/v1) batekin. Bi erdi dira — Python-eko ingestio-pipeline lokal bat (`pipeline/`) eta TypeScript/Hono edge aplikazio bat (`worker/`) — interfaze bakar batean elkartzen direnak: argitaratutako SQL + baliabideen sorta bat.

Ez duzu hodeiko kredentzialik behar laguntzeko. Pipeline-aren muineko moduluak stdlib-ekoak soilik dira eta Worker-aren testak memorian dagoen biltegiratze baten aurka exekutatzen dira.

### Konfigurazioa

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Laguntza non den baliagarriena

**i18n / lokalizazioa** — `worker/src/i18n/ui-strings.json` da UI kateen iturburua. Jatorrizko hiztun batek ingelesa ez den edozein lokaleren berrikuspena oso baliotsua da: makinaren irteera traketsak harrapatu, RTL/diseinu arazoak konpondu, hizkuntza-negoziazioaren kasu bereziak hobetu.

**OCR kalitatea** — OCR egin aurretik idazmakinaz idatzitako eskaneatze zaharren aurre-prozesamendu hobea; kode irekiko motorra eta Tesseract-en fallback-a lagin-orrietan alderatzen dituen ebaluazio-harness bat.

**Irisgarritasuna** — errendatutako orriak (`worker/src/render/`) WCAG-ren aurka auditatu; CSP zorrotza da (`unsafe-inline` gabe), beraz, soluzioek horren barruan funtzionatu behar dute.

**API ergonomia** — `worker/src/routes/` — paginazioa, iragazketa, OpenAPI deskribapena, bezero adibideak.

**Pipeline-aren sendotasuna** — degradazio leuneko bide gehiago, aurrerapenaren berri emate hobea, delta-detekzioaren kasu bereziak (`pipeline/lib/delta.py`).

**Dokumentazioa** — `docs/20260511/` (繁體中文; `00-*` da aurkibidea). Diseinu-dokumentuen ingelesezko itzulpenak ongi etorriak dira.

### Oinarrizko arauak

- Bide guztiak erlatiboak — proiektuak makinen artean eramangarria izan behar du. Ez bide-izen absolutu gogor kodeturik.
- Ez gehitu pip menpekotasunik pipeline-aren *muineko* modulu bati. Aukerako faseek aukerako paketeak erabil ditzakete, eta haiek gabe ondo degradatu behar dute.
- Ez ahuldu aurreranzko soilik den egoera-makina — hori da kostuaren muga.
- Ez sartu AEBetako gobernuaren intsignia ofizialik, eta ez gehitu iturburuko erredakzioak lehengoratzen dituen ezer.
- D1 eskemaren aldaketek **bi** fitxategi ukitzen dituzte: `pipeline/lib/manifest_schema.sql` eta `db/schema.sql`.
- Testak kode berriarekin. Conventional-commit mezuak.

Irakurri `CLAUDE.md` eta `docs/20260511/00-*` lehenik, eta gero ireki issue bat egiturazko edozer eztabaidatzeko PR-a egin aurretik.
