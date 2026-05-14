# GitHub — Post 3 de 3 · Nótaí ailtireachta (Plé i stíl ADR)

**Úsáid mar:** Plé faoi "Taispeáin agus inis" / "Ailtireacht", nó síol `docs/` ADR.
**Eochairfhocail:** ailtireacht, ADR, meaisín staide chun tosaigh amháin, LLM áitiúil, Ollama, OCR, ríomhaireacht imeallach, CSP, ceanntásca slándála, píblíne sonraí, innealtóireacht costais, forléiriú SQLite, D1, R2, KV
**Hipirnaisc:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## An fáth a bhfuil ufolens.com tógtha mar atá sé

Nótaí ar na trí chinneadh a mhúnlaigh [ufolens.com](https://www.ufolens.com) (an atógáil in-chuardaithe, ilteangach ar chartlann [PURSUE UAP](https://www.war.gov/ufo)). Fáilte roimh thráchtanna / brú ar ais.

### 1. Is meaisín staide chun tosaigh amháin í an phíblíne — d'aon ghnó

Staideanna: `aimsithe → íoslódáilte → ocr_déanta → aistrithe → foilsithe`. Ní ghluaiseann doiciméad ach ar aghaidh, agus sin amháin nuair atá obair le déanamh. Ní dhéantar ábhar foilsithe a athphróiseáil go deo mura bhfeiceann brathadóir difríochta gur athraigh an fhoinse i ndáiríre.

**An fáth:** Is iad OCR + aistriúchán na hoibríochtaí costasacha, agus fásann an chartlann le himeacht ama. Tá costas neamhtheoranta ag baint le píblíne a "rithfidh gach rud arís ar mhaithe le cinnteacht". Trí aistrithe ar gcúl a dhéanamh dodhéanta, déantar bille as smacht dodhéanta. Is airí de ghraf an staid an uasteorainn chostais, ní de airdeall an oibritheora.

**An costas:** tá sé deacair d'aon ghnó imirce scéimre agus athphróiseáil d'aon ghnó a dhéanamh. Comhbhabhtáil inghlactha.

### 2. Ritheann OCR agus aistriúchán ar LLM áitiúil, ní ar API néil

OCR: inneall foinse oscailte, cúltaca Tesseract CLI. Aistriúchán + NER: Gemma trí Ollama, ar ríomhaire glúine Apple Silicon.

**An fáth:** costas imeallach nialasach in aghaidh an doiciméid; in-atáirgthe (múnla seasta + leideanna); agus caithfidh an chéim 'fetch' rith ó sheoladh IP cónaithe cheana féin (tá an fhoinse taobh thiar de Akamai Bot Manager — faigheann `curl` 403), mar sin tá ríomhaire glúine sa lúb ar aon nós.

**An costas:** tá cáilíocht an aistriúcháin níos ísle ná múnla ceannródaíoch. I gcás corpas tagartha ina bhfuil an bun-Bhéarla i gcónaí cliceáil amháin ar shiúl, tá sin go breá. Ní mhaímid go bhfuil na haistriúcháin údarásach.

### 3. Níl ach comhéadan amháin ag an dá leath: beartán foilsithe

Ní scríobhann an phíblíne go díreach chuig an mbunachar sonraí táirgeachta go deo. Astaíonn sé `{ SQL, forléiriú sócmhainní, liosta glanta taisce }`. "Foilsiú" = an beartán sin a chur i bhfeidhm ar aghaidh (SQL a bhrú chuig an mbunachar sonraí SQL imeallach, sócmhainní a shioncronú le stóráil oibiachtaí, na heochracha taisce ainmnithe a ghlanadh).

**An fáth:** is féidir leis an taobh áitiúil agus an taobh imeallach forbairt go neamhspleách; is féidir an beartán a athbhreithniú; agus tá an cruth céanna ar "sonraí a imlonnú" gach uair. Is aip beag TypeScript/Hono é an Worker — CSP dian (gan `unsafe-inline`; sha256-pinnáilte ar JSON-LD inlíne), idirbheartaíocht `Accept-Language` + tír→teanga, taisce leathanaigh KV 30-lá, cron laethúil cothabhála — agus ní gá dó a fhios a bheith aige conas a rinneadh na sonraí.

**An costas:** bíonn baint ag athrú scéimre D1 le dhá chomhad (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Árachas saor.

### Neamh-inidirbheartaithe bácáilte isteach san iompar

- Gan a bheith cleamhnaithe le rialtas na S.A.; gan aon suaitheantas oifigiúil.
- Caomhnaítear cealuithe foinse, ní aisiompaítear go deo iad.
- Físeán curtha i leith DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ar fud an tsuímh — innéacsaithe ag innill chuardaigh, rogha an diúltaithe do scríobadh AI.

Beo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

