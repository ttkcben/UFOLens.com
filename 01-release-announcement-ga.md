# GitHub — Post 1 de 3 · Fógra Eisiúna / bloc fógraíochta README

**Úsáid mar:** corp Eisiúna GitHub, Plé pinnáilte, nó barr an repo README.
**Eochairfhocail:** UAP, UFO, cartlann PURSUE, doiciméid dí-aicmithe, sonraí oscailte, cuardach lán-téacs, OCR, aistriúchán uathoibríoch, LLM áitiúil, Ollama, ríomhaireacht imeallach, API poiblí, Hono, TypeScript, Python
**Hipirnaisc:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — ardán ilteangach in-chuardaithe do chartlann PURSUE UAP

**Beo:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Cartlann foinse:** https://www.war.gov/ufo

Déanann `ufolens.com` athfhoilsiú ar chartlann **PURSUE** de chuid Roinn Cogaidh na S.A. de thaifid dí-aicmithe UAP / UFO mar ardán eolais: cuardach lán-téacs, aistriúchán uathoibríoch ar fud an chorpas, taiscéalaíocht léarscáile + amlíne, agus API poiblí JSON. Is saothair de chuid rialtas feidearálach na S.A. iad na doiciméid foinse agus tá siad san fhearann poiblí laistigh de na S.A. ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). **Níl an tionscadal seo cleamhnaithe le rialtas na S.A.**, ní úsáideann sé aon suaitheantas oifigiúil, agus ní dhéanann sé aon fhaisnéis chealaithe a aisiompú.

### Ailtireacht

```
Meaisín áitiúil (Apple Silicon, seoladh IP cónaithe) Líonra imeallach
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, croí-leabharlann chaighdeánach amháin) worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (chun tosaigh amháin) /{lang}/...   leathanaigh
  OCR: inneall foinse oscailte (Tesseract CLI mar chúltaca) /api/v1/...   API poiblí
  translate / NER: LLM áitiúil (Gemma trí Ollama)        /admin        consól oibritheora
  staid: forléiriú SQLite                             le tacaíocht ó: bunachar sonraí SQL imeallach, stóráil
        │                                              oibiachtaí (PDFanna foinse), taisce KV
        └── foilsíonn sé beartán: SQL + forléiriú sócmhainní + liosta glanta taisce ──┘
```

- **Costas nialasach néal-AI in aghaidh an doiciméid.** Ritheann OCR agus aistriúchán go háitiúil; cinntíonn an meaisín staide nach féidir ach dul ar aghaidh (`aimsithe → íoslódáilte → ocr_déanta → aistrithe → foilsithe`) nach ndéantar aon doiciméad a athphróiseáil mura n-athraíonn sé.
- **Níl aon spleáchas ar thríú páirtí ag croí na píblíne** — ritheann agus tástálann modúil anailíse / forléirithe / difríochta ar Python glan gan aon ní pip-shuiteáilte; díghrádaíonn céimeanna OCR/aistriúcháin go galánta nuair a bhíonn pacáistí roghnacha as láthair.
- **Cuireann an suíomh imeallach** ceanntásca slándála dochta + CSP i bhfeidhm (gan `unsafe-inline`; sha256-pinnáilte ar JSON-LD inlíne), idirbheartaíocht teanga trí `Accept-Language` + mapáil tíre, taisce leathanaigh KV 30-lá, agus cron laethúil cothabhála.
- **Nuashonruithe incriminteacha:** aimsíonn brathadóir difríochta an t-innéacs foinse agus ní chuireann sé ach na hathruithe ar ais isteach sa phíblíne.

### Do Fhorbróirí

Tugann an API poiblí ag https://www.ufolens.com/api/v1 doiciméid agus meiteashonraí ar ais mar JSON. Tá rochtain gan ainm teoranta ó thaobh rátaí; iarr eochair do shraitheanna taighdeora/forbróra. Féach an rannóg API ar an suíomh le haghaidh críochphointí agus teorainneacha.

### Stádas

Cód críochnaithe; suíomh imlonnaithe ag https://www.ufolens.com. Líontar an bunachar sonraí táirgeachta tríd an bpíblíne as líne a rith agus an beartán a fhoilsiú ar aghaidh (`cli_publish run --remote`). Tá doiciméid dearaidh iomlána le fáil in `docs/20260511/`.

### Ceadúnas / teorainneacha

- Doiciméid foinse: Saothair de chuid rialtas feidearálach na S.A., fearann poiblí laistigh de na S.A.
- Cód an ardáin seo féin: féach `LICENSE`.
- Seolann an suíomh `Tdm-Reservation: 1` agus `X-Robots-Tag: noai, noimageai` — innéacsaithe ag innill chuardaigh, rogha an diúltaithe do thraenáil/scríobadh AI.
- Cuirtear píosaí scannáin físe i leith DVIDS / AARO agus ní éilíonn an tionscadal seo iad.

Fáilte roimh cheisteanna agus PRs. Léigh `CLAUDE.md` agus `docs/20260511/00-*` le do thoil sula n-osclaíonn tú athruithe struchtúracha.

