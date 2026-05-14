# GitHub — Ingingo 3 kuri 3 · Inyandiko z'imiterere y'ubwubatsi (Ikiganiro cy'uburyo bwa ADR)

**Rikoreshwe nka:** Ikiganiro kiri munsi ya "Erekana kandi ubwire" / "Imiterere y'ubwubatsi", cyangwa intango ya ADR ya `docs/`.
**Amagambo y'ingenzi:** imiterere y'ubwubatsi, ADR, imashini y'imiterere igana imbere gusa, LLM y'imbere mu gihugu, Ollama, OCR, edge computing, CSP, umutwe w'umutekano, umuyoboro w'amakuru, ubuhanga mu kugabanya ibiciro, manifesite ya SQLite, D1, R2, KV
**Imiyoboro:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Impamvu ufolens.com yubatse uko iri

Inyandiko ku byemezo bitatu by'ingenzi byashushanyije [ufolens.com](https://www.ufolens.com) (gusubiramo gushobora gushakishwamo, kw'indimi nyinshi kw'ububiko bwa [PURSUE UAP](https://www.war.gov/ufo)). Ibitekerezo / impaka byakiranwa yombi.

### 1. Umuyoboro ni imashini y'imiterere igana imbere gusa — ku bushake

Imiterere: `yavumbuwe → yakuweho → ocr_yakozwe → yasemuwe → yasohowe`. Inyandiko igenda imbere gusa, kandi gusa iyo hari akazi ko gukora. Ibirimo byasohowe ntibisubirwamo keretse iyo igikoresho cyo gutahura delta kibonye ko inkomoko yahindutse.

**Impamvu:** OCR + ubusemuzi nibyo bikorwa bihenze, kandi ububiko bugenda bwiyongera uko igihe gihita. Umuyoboro "usubiramo byose kugira ngo umutekano ube wizewe" ufite igiciro kitagira umupaka. Gutuma gusubira inyuma bidashoboka bituma fagitire idashobora kwiyongera bidashoboka. Igiciro ntarengwa ni umwihariko w'imiterere y'igishushanyo, ntabwo ari ubushishozi bw'umukoresha.

**Igiciro:** guhindura imiterere no gusubiramo ku bushake biragoye nkana. Ni igihombo cyemewe.

### 2. OCR n'ubusemuzi bikorerwa kuri LLM y'imbere mu gihugu, ntabwo ari kuri API yo mu gicu

OCR: moteri ya open-source, Tesseract CLI nk'uburyo bwisumbuye. Ubusemuzi + NER: Gemma binyuze kuri Ollama, kuri mudasobwa ya Apple Silicon.

**Impamvu:** igiciro cy'inyongera cya zeru kuri buri nyandiko; bishobora gusubirwamo (moderi ihamye + amabwiriza); kandi icyiciro cyo gukurura kigomba gukorerwa kuri IP yo mu rugo (inkomoko iri inyuma ya Akamai Bot Manager — `curl` ihabwa 403), bityo mudasobwa iba iri mu nzira uko byagenda kose.

**Igiciro:** ubwiza bw'ubusemuzi buri munsi y'ubw'imodeli y'icyitegererezo. Ku bubiko bw'icyitegererezo aho Icyongereza cy'umwimerere gihora gishobora kuboneka mu kanya gato, ibyo ni byiza. Ntabwo tuvuga ko ubusemuzi bwacu ari ubw'ukuri.

### 3. Ibice bibiri bisangiye umuyoboro umwe gusa: umuzingo wasohowe

Umuyoboro ntiwigera wandika muri banki y'amakuru y'umusaruro mu buryo butaziguye. Usohora `{ SQL, manifesite y'umutungo, urutonde rwo gusiba cache }`. "Gusohora" = gukoresha uwo muzingo ujya imbere (gushyira SQL muri banki y'amakuru ya SQL y'impera, guhuza umutungo n'ububiko bw'ibintu, gusiba imfunguzo za cache zavuzwe).

**Impamvu:** uruhande rw'imbere mu gihugu n'uruhande rw'impera bishobora guhinduka byigenga; umuzingo ushobora gusuzumwa; kandi "gusohora amakuru" bifite ishusho imwe buri gihe. Worker ni porogaramu ntoya ya TypeScript/Hono — CSP ikomeye (nta `unsafe-inline`; JSON-LD yo mu murongo yashyizweho ikimenyetso cya sha256), `Accept-Language` + guhuza igihugu n'ururimi, cache y'urupapuro ya KV y'iminsi 30, cron yo gusukura ya buri munsi — kandi ntiyigera ikenera kumenya uko amakuru yakozwe.

**Igiciro:** impinduka y'imiterere ya D1 ikora ku fayili ebyiri (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Ubwishingizi buhendutse.

### Ibidahinduka byashyizwe mu myitwarire

- Nta sano na leta ya Leta Zunze Ubumwe z’Amerika; nta birango bya leta.
- Ibyahishwe mu nkomoko birabungabungwa, ntibihindurwa.
- Amavidewo yitirirwa DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ku rubuga rwose — rushobora gushakishwa n'imashini zishakisha, rwavanywemo mu gukusanya amakuru n'ubwenge buhimbano.

Kuri murandasi: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

