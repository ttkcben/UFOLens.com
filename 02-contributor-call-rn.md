# GitHub — Icahuri 2 c'ibitatu · Guhamagarira abaterera / "ibibazo vyiza vyo gutangurirako"

**Koresha nka:** ikiyago c'amanitswe ("Guterera n'ibibazo vyiza vyo gutangurirako") canke intango ya CONTRIBUTING.md.
**Amajambo y'ipfunguruzo:** inkomoko ifunguye, guterera, ikibazo ciza co gutangurirako, i18n, guhindura mu ndimi, OCR, Python, TypeScript, Vitest, pytest, kwinjira, UAP, amakuru rusange
**Amahuza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Guterera kuri ufolens.com

[ufolens.com](https://www.ufolens.com) ihindura [ububiko bwa PURSUE UAP bw'Ishami ry'Ingwano rya Leta Zunze Ubumwe za Amerika](https://www.war.gov/ufo) gushika bube urubuga rushobora gushakishwa, rw'indimi nyinshi rufise [API rusange](https://www.ufolens.com/api/v1). Rigizwe n'ibice bibiri — pipeline yo kwinjiza ya Python yo muhira (`pipeline/`) n'app yo ku mpera ya TypeScript/Hono (`worker/`) — bihura ku kintu kimwe: umugwi wasohotse wa SQL + umutungo.

Nta makuru y'ukwinjira mu gicu ukeneye kugira uterere. Amamodule y'umutima wa pipeline ni stdlib-gusa kandi ibigeragezo vya Worker bikorera ku bubiko bwo mu bwenge.

### Gushiraho

```bash
# pipeline
python3 -m pytest pipeline/tests/          # vyose bikwiye kuba bitoto, ata pip install ikenewe

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Aho imfashanyo ikenewe cane

**i18n / guhindura mu ndimi** — `worker/src/i18n/ui-strings.json` ni isoko ry'amajambo y'interineti. Usubirwamwo n'umuntu avuga ururimi kavukire rw'akarere ako ariko kose katari icongereza ni agaciro kanini: fata ibisohoka bidahuye vy'imashini, kosora ingorane za RTL/uburyo bwo gushira, teza imbere ingorane zo guhuza indimi.

**Ubwiza bwa OCR** — gutunganya neza kuruta ibisikanuro vya kera vyandikishijwe imashini imbere ya OCR; igikoresho co gusuzuma kigereranya moteri y'inkomoko ifunguye na Tesseract nk'insubirizi ku mpapuro z'akarorero.

**Kwinjira** — suzuma impapuro zasohotse (`worker/src/render/`) ugereranije na WCAG; CSP ni ikomeye (ata `unsafe-inline`), rero inyishu zigomba gukorera muri iyo mbibe.

**Ergonomie y'API** — `worker/src/routes/` — gutondekanya impapuro, kuyungurura, ibisobanuro vya OpenAPI, abakiriya b'akarorero.

**Gukomera kwa Pipeline** — inzira nyinshi zo gukora neza naho hari ibibazo, kurusha gutanga raporo y'iterambere, ingorane zo kumenya itandukaniro (`pipeline/lib/delta.py`).

**Inyandiko** — `docs/20260511/` (繁體中文; `00-*` n'urutonde). Ubuhinduzi bw'inyandiko z'umugambi mu congereza murahawe ikaze.

### Amategeko shingiro

- Inzira zose zijanye n'aho ziri — umugambi ugomba kuba ushobora kwimurwa ku mashini zose. Nta nzira zishinze imizi zanditswe.
- Ntiwongere pip dependency kuri module ya *umutima* wa pipeline. Ivyiciro bitari ngombwa bishobora gukoresha amapaki atari ngombwa, kandi bigomba gukora neza naho atahari.
- Ntiworoshe imashini y'ivyiciro ija imbere gusa — iyo n'imbibe y'ikiguzi.
- Ntiwongere ibimenyetso vyemewe vya leta ya Amerika, kandi ntiwongere ikintu na kimwe gihindura ibyahishijwe mu nkomoko.
- Impinduka za D1 schema zikora ku fayili **zibiri**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Ibigeragezo bifise kode nshasha. Ubutumwa bwo kwemeza impinduka bukurikiza amategeko.

Soma `CLAUDE.md` na `docs/20260511/00-*` ubanze, hanyuma wugurure ikibazo kugira muganire ku kintu na kimwe gikomeye imbere ya PR.

