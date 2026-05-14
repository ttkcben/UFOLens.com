# GitHub — Post 3 of 3 · Dintlha tša peakanyo (Discussion ya mokgwa wa ADR)

**Šomiša bjalo ka:** Discussion ka fase ga "Bontšha le go botša" / "Peakanyo", goba peu ya `docs/` ADR.
**Mantšu a bohlokwa:** peakanyo, ADR, motšhene wa boemo bja go ya pele fela, LLM ya selegae, Ollama, OCR, khomphutha ya marangrang, CSP, dihlogo tša tšhireletšo, pipeline ya data, boenjineri bja ditshenyagalelo, manifesete ya SQLite, D1, R2, KV
**Dikgokagano tša inthanete:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Lebaka leo ufolens.com e agilwego ka tsela ye e lego ka yona

Dintlha ka ga diphetho tše tharo tšeo di bopilego [ufolens.com](https://www.ufolens.com) (go agwa lefsa go go kgonago go nyakišišwa, ga maleme a mantši ga [polokelo ya PURSUE UAP](https://www.war.gov/ufo)). Ditshwaotshwao / kganyetšo di amogetšwe.

### 1. Pipeline ke motšhene wa boemo bja go ya pele fela — ka maikemišetšo

Maemo: `o utolotšwe → o laišitšwe → ocr_done → o fetoletšwe → o phatlaladitšwe`. Tokomane e sepela pele fela, gomme fela ge go na le mošomo wo o swanetšego go dirwa. Dikagare tšeo di phatlaladitšwego ga di ke di šongwa gape ntle le ge sedimoši sa phapano se bona gore mothopo o fetošitšwe gabotse.

**Lebaka:** OCR + phetolelo ke mediro yeo e turago, gomme polokelo e gola ge nako e dutše e eya. Pipeline yeo e "phethagatšago gape sengwe le sengwe go netefatša polokego" e na le ditshenyagalelo tšeo di sa laolegego. Go dira gore diphetogo tša morago di se kgonege go dira gore bili yeo e sa laolegego e se kgonege. Ditshenyagalelo tša godimo ke thepa ya kerfi ya boemo, e sego ya go itlhokomela ga modiriši.

**Ditshenyagalelo:** go huduga ga schema le go šoma gape ka maikemišetšo go thata ka boomo. Tshepedišo yeo e amogelegago.

### 2. OCR le phetolelo di sepetšwa go LLM ya selegae, e sego go API ya leru

OCR: enjene ya mothopo o bulegilego, Tesseract CLI fallback. Phetolelo + NER: Gemma ka Ollama, go laptop ya Apple Silicon.

**Lebaka:** ditshenyagalelo tša zero ka tokomane; e kgona go tšweletšwa gape (mohlala wo o sa fetogego + ditaelo); gomme kgato ya go tšea data e šetše e swanetše go sepetšwa go tšwa go IP ya bodulo (mothopo o ka morago ga Akamai Bot Manager — `curl` e hwetša 403), ka fao laptop e ka gare ga tshepedišo.

**Ditshenyagalelo:** boleng bja phetolelo bo ka fase ga mohlala wa maemo a godimo. Bakeng sa corpus ya seswantšho moo Seisemane sa mathomo se lego kgakala ka go kgotla se tee fela, seo se lokile. Ga re bolele gore diphetolelo di na le matla.

### 3. Diripa tše pedi di abelana sebopego se tee fela: sephuthelwana se se phatlaladitšwego

Pipeline ga e ke e ngwala ka go lebanya go polokelongtshedimošo ya tšweletšo. E ntšha `{ SQL, manifesete ya thepa, lenaneo la go hlwekiša sebaka sa polokelo }`. "Go phatlalatša" = šomiša sephuthelwana seo pele (tšea SQL go polokelongtshedimošo ya SQL ya edge, amahanya didirišwa le polokelo ya didirišwa, hlwekiša dinotlelo tša sebaka sa polokelo tšeo di boletšwego).

**Lebaka:** lehlakore la selegae le lehlakore la edge di ka gola ka go ikemela; sephuthelwana se a kgona go hlahlobja; gomme "data ya go romela" e na le sebopego se se swanago nako le nako. Worker ke app ye nnyane ya TypeScript/Hono — CSP ye tiilego (ga go na `unsafe-inline`; JSON-LD ya ka gare e khomareditšwe ka sha256), therisano ya `Accept-Language` + naga→polelo, sebaka sa polokelo sa letlakala sa KV sa matšatši a 30, cron ya go hlwekiša ya letšatši le letšatši — gomme ga e ke e hloka go tseba gore data e dirilwe bjang.

**Ditshenyagalelo:** phetogo ya schema ya D1 e ama difaele tše pedi (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Tšhireletšo ya theko ya fase.

### Dilo tšeo di sa kwanantšwego tšeo di kentšwego boitshwarong

- Ga e amane le mmušo wa U.S.; ga go na maswao a semmušo.
- Diphetošo tša mathomo di a bolokwa, ga di ke di bušetšwa morago.
- Bidio e bolelwa gore ke ya DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` go putla sebaka sa marangrang — e kgona go nyakišišwa, e kgethile go tšwa go kgoboketšo ya AI.

E a phela: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
