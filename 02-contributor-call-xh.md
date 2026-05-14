# GitHub — IsiKhankanya 2 kwezi-3 · Umnqophiso wabaxhasi / "imiba emihle yokuqala"

**Sisebenzise njenge:** i-Discussion ebambekileyo ("Ukuxhasa kunye nemiba emihle yokuqala") okanye isiqalo se-CONTRIBUTING.md.
**Amagama aphambili:** i-open source, ukuxhasa, umba olungileyo wokuqala, i18n, ukwenziwa kwalapha ekhaya, OCR, Python, TypeScript, Vitest, pytest, ukufikeleleka, UAP, idatha evulekileyo
**Iziqhagamshelanisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ukuxhasa ku-ufolens.com

[ufolens.com](https://www.ufolens.com) iguqula ingobo yovimba yeSebe leMfazwe lase-U.S. [PURSUE UAP archive](https://www.war.gov/ufo) ibe liqonga elikhangelweyo, leelwimi ezininzi eline [API yoluntu](https://www.ufolens.com/api/v1). Zizixekana ezibini — i-pipeline yasekhaya yokufaka idatha ye-Python (`pipeline/`) kunye ne-TypeScript/Hono edge app (`worker/`) — zidibana kwi-interface enye: i-SQL epapashiweyo + ibundle yezinto.

Awudingi naziphi na iziqinisekiso zefu ukuba uxhase. Iimodeli ezingundoqo ze-pipeline yi-stdlib-only kwaye iimvavanyo ze-Worker zisebenza ngokuchasene nokugcina okwenziwe ngaphakathi kwimemori.

### Ukuseta

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Apho uncedo lunceda kakhulu

**i18n / ukwenziwa kwalapha ekhaya** — `worker/src/i18n/ui-strings.json` ngumthombo weentambo ze-UI. Uphononongo olwenziwe ngumntu othetha ulwimi lwesintu lwayo nayiphi na ilizwe elingelilo lesiNgesi linexabiso eliphezulu: fumana imveliso yomatshini engalunganga, lungisa imiba ye-RTL/layout, phucula iimeko ezimandla zokuthethathethana ngolwimi.

**Umgangatho we-OCR** — ukulungiswa okungcono kweeskhene ezindala ezibhaliweyo ngomatshini phambi kwe-OCR; isixhobo sokuvavanya esithelekisa injini ye-open-source vs. i-Tesseract fallback kumaphepha esampula.

**Ukufikeleleka** — hlola amaphepha ahleliweyo (`worker/src/render/`) ngokuchasene ne-WCAG; i-CSP iqinile (akukho `unsafe-inline`), ngoko izisombululo kufuneka zisebenze ngaphakathi kwayo.

**I-API ergonomics** — `worker/src/routes/` — ukuhambisa amaphepha, ukucoca, inkcazo ye-OpenAPI, iiklasi zomzekelo.

**Ukuqina kwe-Pipeline** — iindlela ezininzi zokuwohloka ngokukhawuleza, ukubika inkqubela phambili okungcono, iimeko ezimandla zokubona idelta (`pipeline/lib/delta.py`).

**Amaxwebhu** — `docs/20260511/` (繁體中文; `00-*` yincwadi yezalathiso). Iinguqulelo zamaxwebhu oyilo kwisiNgesi zamkelekile.

### Imithetho esisiseko

- Zonke iindlela zihambelana — iprojekthi kufuneka ikwazi ukuthuthwa phakathi koomatshini. Akukho zindlela ezicwangciswe ngqo.
- Musa ukongeza ukuxhomekeka kwe-pip kwimodyuli *engundoqo* ye-pipeline. Izigaba ozikhethayo zinokusebenzisa iipakethe ozikhethayo, kwaye kufuneka ziwohloke ngokukhawuleza ngaphandle kwazo.
- Musa ukuyenza buthathaka umshini welizwe olulodwa lokuya phambili — loo yindawo ephezulu yeendleko.
- Musa ukufaka iimpawu zikarhulumente wase-U.S. ezisemthethweni, kwaye musa ukongeza nantoni na eyenza utshintsho olubonakalayo olwenzelwe ukufihla ulwazi.
- Utshintsho lwesikimu se-D1 luchaphazela iifayile **ezimbini**: `pipeline/lib/manifest_schema.sql` kunye `db/schema.sql`.
- Iimvavanyo ngekhowudi entsha. Imiyalezo ye-Conventional-commit.

Funda `CLAUDE.md` kunye `docs/20260511/00-*` kuqala, emva koko uvule umba wokuxoxa ngento ethile ngaphambi kwe-PR.

