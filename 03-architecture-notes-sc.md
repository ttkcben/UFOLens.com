# GitHub — Post 3 de 3 · Notas de architetura (Diskussione in istile ADR)

**Impreu comente:** una Diskussione in "Amosta e conta" / "Architetura", o comente puntu de partida pro un'ADR in `docs/`.
**Faeddos crae:** architetura, ADR, màchina a istados chi andat sceti a in antis, LLM locale, Ollama, OCR, edge computing, CSP, intestatziones de seguridade, pipeline de datos, ingegneria de is costos, manifestu SQLite, D1, R2, KV
**Acàpios:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Pro ite ufolens.com est fatu aici

Notas a pitzu de is tres detzisiones chi ant formadu [ufolens.com](https://www.ufolens.com) (sa ri-creatzione chircàbile e multilìngua de s'archiviu [PURSUE UAP](https://www.war.gov/ufo)). Cumentos e crìticas sunt bene bènnidos.

### 1. Sa pipeline est una màchina a istados chi andat sceti a in antis — pro isseberu

Istados: `iscobertu → iscarrigadu → ocr_acabadu → traduìdu → publicadu`. Unu documentu si movet sceti a in antis, e sceti cando ddoe at traballu de fàghere. Su cuntènnidu publicadu no est mai torradu a protzessare si unu rilevadore de delta no bit chi s'originale est cambiadu a beru.

**Pro ite:** s'OCR + sa tradutzione sunt is operatziones prus costosas, e s'archiviu creschet cun su tempus. Una pipeline chi "torrat a esecutare totu pro seguresa" tenet unu costu sena lìmites. Rèndere is transitziones a s'segus impossìbiles faghet impossìbile una spesa sena controllu. Su lìimite de costu est una propiedade de su gràficu de is istados, no de sa vigilàntzia de s'operadore.

**Costu:** is migratziones de s'ischema e su ri-protzessamentu fatu a posta sunt pagu pràticos. Unu cumpromissu atzetàbile.

### 2. S'OCR e sa tradutzione funtzionant in unu LLM locale, no in una API in su cloud

OCR: motore open-source, Tesseract CLI comente alternativa. Tradutzione + NER: Gemma bia Ollama, in unu laptop Apple Silicon.

**Pro ite:** costu marginale zero pro documentu; riproduìbile (modellu + prompts fissos); e sa fase de achirimentu giai depet funtzionare dae un'IP residentziale (s'originale est a palas de Akamai Bot Manager — `curl` retzit unu 403), duncas unu laptop serbit de onni manera.

**Costu:** sa calidade de sa tradutzione est prus bàscia de unu modellu de avanguàrdia. Pro unu corpus de riferimentu ue s'originale inglesu est semper a disponimentu cun unu clic, andat bene. No afirmamus chi is tradutziones siant autoritàrias.

### 3. Is duas metades cumpartzint sceti un'interfache: unu pachete publicadu

Sa pipeline no iscrit mai in sa base de datos de produtzione diretamente. Emitit `{ SQL, manifestu de is assets, lista de purga de sa cache }`. "Publicare" cheret nàrrere aplicare cussu pachete a in antis (insertare s'SQL in sa base de datos SQL de s'edge, sincronizare is assets in s'object storage, purgare is craes de cache numenadas).

**Pro ite:** sa parte locale e sa parte in s'edge podent evolvere in manera indipendente; su pachete si podet revisionare; e "distribuire is datos" tenet semper sa matessi forma. Su Worker est un'aplicatzione pitica in TypeScript/Hono — CSP tostu (perunu `unsafe-inline`; su JSON-LD in lìnia est blocadu cun sha256), negotziatzione de `Accept-Language` + natzione→limba, cache de pàgina in KV de 30 dies, cron de mantenimentu giornalieru — e no tenet mai bisòngiu de ischire comente is datos sunt istados creados.

**Costu:** unu càmbio a s'ischema de D1 tocat duos files (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un'asseguratzione barata.

### Règulas non negotziàbiles integradas in su cumportamentu

- No afiliadu cun su guvernu de is Istados Unidos; perunu sìmbulu ufitziale.
- Is partes eliminadas in is documentos originales sunt cunservadas, mai isveladas.
- Vìdeos atribuidos a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` in totu su situ — inditzizàbile dae is motores de chirca, ma s'est esclusu dae s'iscarrigamentu de is AI.

In lìnia: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

