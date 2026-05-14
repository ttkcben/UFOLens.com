# GitHub — Post 2 de 3 · Glao ar rannpháirtithe / "dea-chéad cheisteanna"

**Úsáid mar:** Plé pinnáilte ("Rannpháirtíocht & dea-chéad cheisteanna") nó réamhrá do CONTRIBUTING.md.
**Eochairfhocail:** foinse oscailte, rannpháirtíocht, dea-chéad cheist, i18n, logánú, OCR, Python, TypeScript, Vitest, pytest, inrochtaineacht, UAP, sonraí oscailte
**Hipirnaisc:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ag cur le ufolens.com

Déanann [ufolens.com](https://www.ufolens.com) cartlann [PURSUE UAP](https://www.war.gov/ufo) de chuid Roinn Cogaidh na S.A. a athrú go hardán in-chuardaithe, ilteangach le [API poiblí](https://www.ufolens.com/api/v1). Tá sé comhdhéanta de dhá leath — píblíne ionghabhála áitiúil Python (`pipeline/`) agus aip imeallach TypeScript/Hono (`worker/`) — a thagann le chéile ag comhéadan amháin: beartán foilsithe de SQL + sócmhainní.

Níl aon dintiúir néil ag teastáil uait chun cur leis. Is leabharlann chaighdeánach amháin iad croí-mhodúil na píblíne agus ritheann tástálacha an Worker i gcoinne stórála in-chuimhne.

### Socrú

```bash
# pipeline
python3 -m pytest pipeline/tests/          # ba chóir go mbeadh gach rud glas, níl gá le pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Na réimsí is mó ina bhfuil cabhair ag teastáil

**i18n / logánú** — Is é `worker/src/i18n/ui-strings.json` foinse na sreangán comhéadain. Is mór an luach atá le hathbhreithniú ó chainteoirí dúchais ar aon logán neamh-Bhéarla: chun aschur meaisín míchuí a cheartú, chun fadhbanna leagan amach/RTL a réiteach, agus chun cásanna imeallacha idirbheartaíochta teanga a fheabhsú.

**Cáilíocht OCR** — réamhphróiseáil níos fearr ar shean-scantaí clóscríofa roimh OCR; úim mheastóireachta a dhéanann comparáid idir an t-inneall foinse oscailte agus cúltaca Tesseract ar leathanaigh shamplacha.

**Inrochtaineacht** — iniúchadh ar na leathanaigh rindreáilte (`worker/src/render/`) i gcoinne WCAG; tá an CSP dian (gan `unsafe-inline`), mar sin ní mór do réitigh oibriú laistigh de sin.

**Eirgeanamaíocht API** — `worker/src/routes/` — leathanú, scagadh, cur síos OpenAPI, cliaint shamplacha.

**Stóinseacht na píblíne** — níos mó conairí díghrádaithe galánta, tuairisciú dul chun cinn níos fearr, cásanna imeallacha braite difríochta (`pipeline/lib/delta.py`).

**Doiciméid** — `docs/20260511/` (繁體中文; is é `00-*` an t-innéacs). Fáilte roimh aistriúcháin ar na doiciméid dearaidh go Béarla.

### Bunrialacha

- Gach conair coibhneasta — ní mór don tionscadal a bheith iniompartha idir meaisíní. Gan aon chonairí absalóideacha cruachódaithe.
- Ná cuir spleáchas pip le modúl *lárnach* píblíne. Féadfaidh céimeanna roghnacha pacáistí roghnacha a úsáid, agus ní mór dóibh díghrádú go galánta gan iad.
- Ná lagaigh an meaisín staide nach féidir ach dul ar aghaidh — sin an uasteorainn chostais.
- Ná tabhair isteach suaitheantas oifigiúil rialtas na S.A., agus ná cuir aon rud leis a aisiompaíonn cealuithe foinse.
- Bíonn baint ag athruithe scéimre D1 le **dhá** chomhad: `pipeline/lib/manifest_schema.sql` agus `db/schema.sql`.
- Tástálacha le cód nua. Teachtaireachtaí coinbhinsiúnta-tiomanta.

Léigh `CLAUDE.md` agus `docs/20260511/00-*` ar dtús, ansin oscail ceist chun aon rud struchtúrach a phlé roimh an PR.

