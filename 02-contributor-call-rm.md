# GitHub — Post 2 da 3 · Clom da contribuents / "buns emprims problems"

**Utilisar sco:** ina discussiun fixada ("Contribuziun & buns emprims problems") u in'introducziun en CONTRIBUTING.md.
**Pleds-clav:** open source, contribuir, bun emprim problem, i18n, localisaziun, OCR, Python, TypeScript, Vitest, pytest, accessibilitad, UAP, datas avertas
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuir a ufolens.com

[ufolens.com](https://www.ufolens.com) transfurma l'[archiv PURSUE UAP](https://www.war.gov/ufo) dal Departament da Guerra dals Stadis Unids en ina plattafurma tschertgabla e plurilingua cun in'[API publica](https://www.ufolens.com/api/v1). Igl èn duas mesadads — ina pipeline d'ingestiun locala da Python (`pipeline/`) ed in'applicaziun da l'edge da TypeScript/Hono (`worker/`) — che s'entaupan tar ina suletta interfatscha: in pachet publitgà da SQL + assets.

Vus n'avais betg da basegn da credenzials da cloud per contribuir. Ils moduls principals da la pipeline èn be stdlib e las testas dal Worker funcziuneschan cun ina memoria in-memory.

### Installaziun

```bash
# pipeline
python3 -m pytest pipeline/tests/          # tut duess esser verd, nagin pip install necessari

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Nua che agid è il pli util

**i18n / localisaziun** — `worker/src/i18n/ui-strings.json` è la funtauna dals texts da l'UI. Ina revisiun da pledaders nativs da mintga localisaziun betg englaisa è da gronda valur: chattar translaziuns automaticas malgartegiadas, curreger problems da RTL/layout, meglierar cas da cunfin da la negoziaziun da lingua.

**Qualitad d'OCR** — meglra pre-elavuraziun da vegli scans da scrittira a maschina avant l'OCR; in'infrastructura d'evaluaziun che cumparescha l'engine open-source cun il fallback da Tesseract sin paginas d'exempel.

**Accessibilitad** — auditar las paginas rendidas (`worker/src/render/`) cunter WCAG; il CSP è sever (nagin `unsafe-inline`), pia ston las soluziuns funcziunar entaifer quel.

**Ergonomia da l'API** — `worker/src/routes/` — paginaziun, filtragiun, descripziun OpenAPI, clients d'exempel.

**Robustezza da la pipeline** — dapli vias da degradaziun graziaivla, meglra rapportaziun dal progress, cas da cunfin da la detecziun da delta (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` è l'index). Translaziuns dals documents da design en englais èn bainvegnidas.

### Reglas da basa

- Tut las vias èn relativas — il project sto esser portabel tras maschina. Naginas vias absolutas fixas.
- Betg agiuntar ina dependenza da pip ad in modul *principal* da la pipeline. Etappas opziunals pon utilisar pachets opziunals, e ston degradar cun grazia senza els.
- Betg indeblir la maschina da stadi che va be enavant — quai è il plafun dals custs.
- Betg agiuntar insigns uffizials dal govern dals Stadis Unids, e betg agiuntar insatge che revochescha redacziuns da funtauna.
- Midadas al schema da D1 toccan **dus** files: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Testas cun nov code. Messadis da commit convenziunals.

Legi `CLAUDE.md` e `docs/20260511/00-*` l'emprim, lura avri in problem per discutar insatge structural avant il PR.

