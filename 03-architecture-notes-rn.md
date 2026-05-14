# GitHub — Icahuri 3 c'ibitatu · Inyandiko z'ubwubatsi (Ikiyago c'ubwoko bwa ADR)

**Koresha nka:** Ikiyago munsi ya "Erekana kandi uvuge" / "Ubwubatsi", canke intango ya `docs/` ADR.
**Amajambo y'ipfunguruzo:** ubwubatsi, ADR, imashini y'ivyiciro ija imbere gusa, LLM yo muhira, Ollama, OCR, edge computing, CSP, amategeko y'umutekano, pipeline y'amakuru, ubwubatsi bw'ibiguzi, maniferesite ya SQLite, D1, R2, KV
**Amahuza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Impamvu ufolens.com yubatswe uko iri

Inyandiko ku ngingo zitatu zatumye haba [ufolens.com](https://www.ufolens.com) (isubirwamwo rishobora gushakishwa, ry'indimi nyinshi ry'[ububiko bwa PURSUE UAP](https://www.war.gov/ufo)). Ivyiyumviro / impanuro murahawe ikaze.

### 1. Pipeline ni imashini y'ivyiciro ija imbere gusa — ku bushake

Ivyiciro: `kivumbuwe → cakuwe → ocr_kirangiye → cahinduwe → casohotse`. Inyandiko ija imbere gusa, kandi gusa igihe hari akazi ko gukora. Ibirimwo vyasohotse ntibigera bisubirwamwo kiretse igikoresho kiraba itandukaniro kibonye ko inkomoko yahindutse koko.

**Impamvu:** OCR + ubuhinduzi ni ibikorwa bihenze, kandi ububiko burakura uko igihe gihera. Pipeline "isubiramwo vyose kugira ngo ibe ikekanye" ifise ikiguzi kitagira imbibe. Gutuma kudashoboka gusubira inyuma bituma ifagitire idashoboka gukura. Imbibe y'ikiguzi ni ikiranga igishushanyo c'ivyiciro, si ikiranga ubwitonzi bw'uwukoresha.

**Ikiguzi:** guhindura schema no gusubiramwo ku bushake biragoye ku bushake. Ig tradeoff cemewe.

### 2. OCR n'ubuhinduzi bikorera kuri LLM yo muhira, ntibikoresha API yo ku gicu

OCR: moteri y'inkomoko ifunguye, Tesseract CLI nk'insubirizi. Ubuhinduzi + NER: Gemma biciye kuri Ollama, kuri laptop ya Apple Silicon.

**Impamvu:** ikiguzi c'inyongera c'ubusa ku nyandiko imwe imwe; bishobora gusubirwamwo (imodeli n'amajambo bifatiriye); kandi ikiciro co gukurura gisanzwe kigomba gukorera kuri IP yo mu rugo (inkomoko iri inyuma ya Akamai Bot Manager — `curl` ironka 403), rero laptop irimwo mu gikorwa.

**Ikiguzi:** ubwiza bw'ubuhinduzi buri hasi y'imodeli y'imbere. Ku bubiko bw'akarorero aho icongereza c'inkomoko gihora kiri ku klik imwe, birakunda. Ntituvuga ko ubuhinduzi ari ubwemewe.

### 3. Ibice bibiri bisangiye neza na neza interineti imwe: umugwi wasohotse

Pipeline ntiyigera yandika mu bubiko bw'amakuru bwo mu buhinga butaziguye. Isohora `{ SQL, maniferesite y'umutungo, urutonde rwo guhanagura cache }`. "Gusohora" = gushira mu ngiro uwo mugwi imbere (gushira SQL muri edge SQL DB, guhuza umutungo n'ububiko bw'ibintu, guhanagura amakiyi ya cache yavuzwe).

**Impamvu:** uruhande rwo muhira n'uruhande rwo ku mpera bishobora gutera imbere ukwavyo; umugwi urashobora gusuzumwa; kandi "gushira mu ngiro amakuru" bifise ishusho imwe igihe cose. Worker ni app ntoya ya TypeScript/Hono — CSP ikomeye (ata `unsafe-inline`; inline JSON-LD ifise ikimenyetso ca sha256), `Accept-Language` + guhuza igihugu→ururimi, cache y'impapuro imara imisi 30 kuri KV, cron y'isuku ya minsi yose — kandi ntiyigera ikenera kumenya uko amakuru yakozwe.

**Ikiguzi:** impinduka ya D1 schema ikora ku fayili zibiri (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Ubwishingizi buhendutse.

### Ibidashobora guhinduka vyashizwe mu ngeso

- Ntaho bihuriye na leta ya Amerika; ata bimenyetso vyemewe.
- Ibyahishijwe mu nkomoko birabungwabungwa, ntibigera bihindurwa.
- Amavidewo yavanywe kuri DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` ku rubuga rwose — rushobora gushakishwa mu bishakisha, rwakuyemwo gukururwa na AI.

Ruri ku murongo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
