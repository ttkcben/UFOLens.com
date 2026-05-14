# GitHub — Post 1 of 3 · Phatlalatšo ya go lokollwa / Bloko ya tsebišo ya README

**Šomiša bjalo ka:** mmele wa GitHub Release, Discussion yeo e khomareditšwego, goba godimo ga README ya polokelo.
**Mantšu a bohlokwa:** UAP, UFO, polokelo ya PURSUE, ditokomane tšeo di sa utollwago, data yeo e bulegilego, phenyo ya mongolo ka botlalo, OCR, phetolelo ya motšhene, LLM ya selegae, Ollama, khomphutha ya marangrang, API ya setšhaba, Hono, TypeScript, Python
**Dikgokagano tša inthanete:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — sethalwa sa maleme a mantši, seo se kgonago go nyakišišwa sa polokelo ya PURSUE UAP

**E a phela:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Polokelo ya mathomo:** https://www.war.gov/ufo

`ufolens.com` e phatlalatša gape polokelo ya **PURSUE** ya Lefapha la Ntwa la U.S. ya direkhoto tšeo di sa utollwago tša UAP / UFO bjalo ka sethalwa sa tsebo: phenyo ya mongolo ka botlalo, phetolelo ya motšhene go putla corpus, go hlahloba mmapa + tatelano ya dinako, le API ya JSON ya setšhaba. Ditokomane tša mathomo ke mešomo ya mmušo wa koporasi wa U.S. gomme ka gare ga U.S. ke sebaka sa setšhaba ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Porojeke ye **ga e amane le mmušo wa U.S.**, ga e šomiše maswao a semmušo, gomme ga e ke e bušetša morago diphetošo.

### Peakanyo

```
Motšhene wa selegae (Apple Silicon, IP ya bodulo)   Marangrang a Edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, mokgo wa stdlib-fela)       worker/  (TypeScript, Hono.js)
  go tšea → OCR → go fetolela → go phatlalatša (go ya pele fela)    /{lang}/...   matlakala
  OCR: enjene ya mothopo o bulegilego (Tesseract CLI fallback)     /api/v1/...   API ya setšhaba
  go fetolela / NER: LLM ya selegae (Gemma ka Ollama)        /admin        khonsolo ya modiriši
  boemo: Manifesete ya SQLite                           e thekgwa ke: polokelongtshedimošo ya SQL ya edge, polokelo
        │                                              ya didirišwa (di-PDF tša mathomo), sebaka sa polokelo sa KV
        └── e phatlalatša sephuthelwana: SQL + manifesete ya thepa + lenaneo la go hlwekiša sebaka sa polokelo ──┘
```

- **Ditshenyagalelo tša zero tša AI ya leru ka tokomane.** OCR le phetolelo di sepetšwa selegae; motšhene wa boemo bja go ya pele fela (`o utolotšwe → o laišitšwe → ocr_done → o fetoletšwe → o phatlaladitšwe`) o netefatša gore ga go tokomane yeo e šomago gape ntle le ge e fetošwa.
- **Mokgo wa pipeline ga o na boitšhepo bja mokgatlho wa boraro** — di-module tša go arola / manifesete / phapano di sepela le go leka go Python ye e hlwekilego yeo e se nago selo seo se kentšwego ka pip; dikgato tša OCR/phetolelo di theoga gabotse ge diphuthelwana tša boikgethelo di se gona.
- **Sebaka sa marangrang sa Edge** se šomiša dihlogo tša tšhireletšo tše tiilego + CSP (ga go na `unsafe-inline`; JSON-LD ya ka gare e khomareditšwe ka sha256), therisano ya polelo ka `Accept-Language` + go mapa naga, sebaka sa polokelo sa letlakala sa KV sa matšatši a 30, le cron ya go hlwekiša ya letšatši le letšatši.
- **Dintlafatšo tša go oketša:** sedimoši sa phapano se bapiša lenaneo la mathomo gomme se fepa diphetogo fela morago ka gare ga pipeline.

### Bakeng sa bahlami

API ya setšhaba go https://www.ufolens.com/api/v1 e bušetša ditokomane le metadata bjalo ka JSON. Phihlelelo ya go se tsebjwe e lekanyeditšwe; kgopela senotlelo sa maemo a banyakišiši/bahlami. Bona karolo ya API go sebaka sa marangrang sa inthanete bakeng sa dintlha tša mafelelo le mekgatlo.

### Maemo

Khoutu e feletše; sebaka sa marangrang se rometšwe go https://www.ufolens.com. Polokelongtshedimošo ya tšweletšo e tlatšwa ka go sepetša pipeline ya ka ntle ga inthanete le go phatlalatša sephuthelwana pele (`cli_publish run --remote`). Ditokomane tša peakanyo ka botlalo di dula go `docs/20260511/`.

### Laesense / mellwane

- Ditokomane tša mathomo: Mešomo ya mmušo wa koporasi wa U.S., sebaka sa setšhaba ka gare ga U.S.
- Khoutu ya sethalwa se: bona `LICENSE`.
- Sebaka sa marangrang se romela `Tdm-Reservation: 1` le `X-Robots-Tag: noai, noimageai` — se kgona go nyakišišwa ke dienjine tša phenyo, se kgethile go tšwa go tlwaetšo/go kgoboketša ga AI.
- Diswantšho tša bidio di bolelwa gore ke tša DVIDS / AARO gomme ga di tsejwe ke porojeke ye.

Dintla le di-PR di amogetšwe. Hle bala `CLAUDE.md` le `docs/20260511/00-*` pele o bula diphetogo tša sebopego.

