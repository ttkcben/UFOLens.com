# GitHub — Poosi 3 ʻo e 3 · Ngaahi fakamatalanga ki he ʻApitikitea (Fetalanoaʻaki ʻo e kalasi ADR)

**Fakaʻaongaʻi ʻi he:** ha Fetalanoaʻaki ʻi he "Fakahaaʻi mo fakamatalanga" / "ʻApitikitea", pe ko ha `docs/` ADR seed.
**Ngaahi Lea Mahuʻinga:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Ngaahi Līnki:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ko e ʻuhinga ʻoku langa pehē ai ʻa e ufolens.com

Ngaahi fakamatalanga ki he ngaahi fili ʻe tolu naʻe faʻu ai ʻa e [ufolens.com](https://www.ufolens.com) (ko e toe langa fekumiʻanga, fakalea-laulau ʻo e [tatauʻanga PURSUE UAP](https://www.war.gov/ufo)). ʻOku talitali lelei ʻa e ngaahi fakamatala / ngaahi fakafepaki.

### 1. Ko e pipeline ko ha forward-only state machine — ʻoku tuʻuaki

Ngaahi tuʻunga: `discovered → downloaded → ocr_done → translated → published`. ʻOku ʻalu pē ha tohi ki muʻa, pea ʻoku fakahoko pē ʻo ka ʻi ai ha ngāue ke fai. ʻOku ʻikai ke toe fakaleleiʻi ha kakano kuo fakamafola tuku kehe kapau ʻoku ʻilo ʻe ha delta detector kuo liliu moʻoni ʻa e tuʻati.

**Ko e ʻuhinga:** Ko e OCR + fakatonulea ko e ngaahi ngāue ia ʻoku fuʻu tōtongi, pea ʻoku tupu ʻa e tatauʻanga ʻi he lolotonga ʻo e taimi. Ko ha pipeline ʻoku "toe fakalele kotoa ke malu" ʻoku ʻikai hano tapui ʻi he tōtongi. Ko hono ʻikai malava ke fakahoko ʻa e ngaahi transition ki mui ʻoku ʻikai ai ha foʻi moʻua lahi. Ko e tapui ʻo e tōtongi ko ha ʻulungāanga ia ʻo e state graph, ʻikai ko e leʻo ʻo e operator.

**Tōtongi:** ko e schema migrations pea mo e toe fakaleleiʻi ʻoku tuʻuaki ʻoku fuʻu faingataʻa. Ko ha tradeoff ʻoku tali lelei.

### 2. Ko e OCR mo e fakatonulea ʻoku fakahoko ʻi ha local LLM, ʻikai ko ha cloud API

OCR: open-source engine, Tesseract CLI fallback. Fakatonulea + NER: Gemma ʻo fakafou ʻi he Ollama, ʻi ha Apple Silicon laptop.

**Ko e ʻuhinga:** taʻe-tōtongi fakalahi ki he tohi takitaha; lava ke toe fakalele (fixed model + prompts); pea ko e fetch step ʻoku kuo pau ke fakalele ia mei ha IP ʻi he nofoʻanga (ko e tuʻati ʻoku ʻi mui ʻi he Akamai Bot Manager — ʻoku maʻu ʻe he `curl` ha 403), ko ia ko ha laptop ʻoku ʻi he loop pē.

**Tōtongi:** ko e lelei ʻo e fakatonulea ʻoku ʻi lalo ʻi ha frontier model. Ki ha reference corpus ʻa ia ʻoku maʻu ai ʻa e Faka-Pilitānia fakatuʻati ʻi ha kiliki pē taha, ʻoku lelei ia. ʻOku ʻikai ke mau faʻu ko e ngaahi fakatonulea ʻoku fakamoʻoni.

### 3. Ko e ngaahi konga ʻe ua ʻoku nau vahevahe ha interface ʻe taha pē: ko ha published bundle

ʻOku ʻikai ke tohi ʻa e pipeline ki he production database fakahangatonu. ʻOku fakaʻasi ʻe ia `{ SQL, asset manifest, cache-purge list }`. "Fakamafola" = fakaʻaongaʻi ʻa e bundle ko ia ki muʻa (lomiʻi ʻa e SQL ki he edge SQL DB, sync ʻa e ngaahi asset ki he object storage, fakamaʻa ʻa e ngaahi cache key kuo fakahingoa).

**Ko e ʻuhinga:** ʻe lava ke tupu fakafoʻituitui ʻa e local side mo e edge side; ʻe lava ke vakaiʻi ʻa e bundle; pea ko e "deploy data" ko e tatau tatau pē ʻi he taimi kotoa pē. Ko e Worker ko ha kiʻi TypeScript/Hono app — CSP mālohi (ʻikai ha `unsafe-inline`; inline JSON-LD ʻoku sha256-pinned), `Accept-Language` + fetalanoaʻaki ʻi he lea ʻo e fonua, 30-day KV page cache, cron fakaʻaho ki he fakamaʻa — pea ʻoku ʻikai ke fiemaʻu ia ke ʻilo pe naʻe faʻu fēfē ʻa e data.

**Tōtongi:** ko ha D1 schema change ʻoku ala ki he ngaahi tohi ʻe ua (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Ko ha inisiua maʻamaʻa.

### Ngaahi meʻa ʻoku ʻikai lava ke fetongi ʻoku ʻi he ʻulungāanga

- ʻOku ʻikai ke kaungā ngāue mo e puleʻanga ʻo e U.S.; ʻoku ʻikai ha fakaʻilonga fakapuleʻanga.
- ʻOku maluʻi ʻa e ngaahi fakamālohia fakatuʻati, ʻoku ʻikai ke fakafepakiʻi.
- Ko e vitiō ʻoku fakafehokotaki ki he DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ʻi he saiti kotoa — lava ke fikaʻi ʻi he fekumi, kuo fili ke ʻikai kau ʻi he toʻo fakatonulea AI.

Maʻu: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
