# GitHub — Nyatakaka 3 le 3 me · Nusɔsrɔ̃ ƒe Ðoɖo ŋuti nyawo (ADR-style Nyamedzro)

**Zã abe:** Nyamedzro le "Nu Fiafia" / "Nusɔsrɔ̃ ƒe Ðoɖo" te, alo `docs/` ADR ƒe gɔmedzenu.
**Nya veviwo:** nusɔsrɔ̃ ƒe ɖoɖo, ADR, ŋgɔ-yiyi-ɖeɖe ƒe nɔnɔme mɔ̃, local LLM, Ollama, OCR, edge computing, CSP, dedienɔnɔ ƒe sededewo, data pipeline, ga-zazã ƒe ɖoɖo, SQLite manifest, D1, R2, KV
**Kadodo veviwo:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Nusi-ta wo-tu ufolens.com ɖo alea

Nya-ɖewo le tiatia etɔ̃ siwo na [ufolens.com](https://www.ufolens.com) (gbe-ge-ɖewo me gbugbɔtu si me wo-teaŋu di-na nu le na [PURSUE UAP archive](https://www.war.gov/ufo)). Mía-kpɔ dzidzɔ ɖe mia-ƒe nya-wo kple nya-gbugbɔgblɔ̃wo ŋu.

### 1. Pipeline la nye ŋgɔ-yiyi-ɖeɖe ƒe nɔnɔme mɔ̃ — le susu aɖe ta

Nɔnɔmewo: `discovered → downloaded → ocr_done → translated → published`. Agbalẽ ɖeka-ɖeka yia ŋgɔ ko, eye ne dɔ li wò-awɔ ko hafi. Wo-me-gbugbɔa nu-siwo wo-ta la ƒe dɔ wɔna gbeɖe o negbe ɖe delta detector aɖe kpɔe be gɔmedzenua trɔ vavã.

**Nusi-tae:** OCR + gɔmeɖeɖe nye dɔwɔna siwo xɔa ga, eye archive la le dzidzim ɖe edzi. Pipeline si "gbugbɔa nuwo katã wɔna be woa-kɔ" la he-a ga-zazã si nu meli na o vɛ. Ne wo-wɔe be megbe-yiyi me-li o la, ga-gbegblẽ gã aɖeke ma-dzɔ gbeɖe o. Ga-si woa-zã ƒe se-ɖoƒea nyrɔ ɖe nɔnɔme graph la me, ke menye le nu-wɔla ƒe ŋudzɔnɔnɔ me o.

**Gadzɔ-gadzɔ:** schema ƒe tɔtrɔwo kple gbugbɔ-wɔwɔ tɔxɛwo sesẽ na dɔwɔwɔ. Asi-tsɔtsɔ si sɔ.

### 2. OCR kple gɔmeɖeɖe zɔna le local LLM dzi, ke menye le cloud API dzi o

OCR: open-source engine, Tesseract CLI ƒe kpeɖeŋu. Gɔmeɖeɖe + NER: Gemma to Ollama dzi, le Apple Silicon laptop dzi.

**Nusi-tae:** ga-hake aɖeke me-nɔa agbalẽ ɖesiaɖe ŋu o; wo-teaŋu gbugbɔa wɔna (model + prompts ma-trɔna); eye fetch dɔwɔna la hiã be woa-wɔe tso aƒeme IP dzi xoxo (gɔmedzenu la le Akamai Bot Manager megbe — `curl` xɔa 403), eyata laptop la le dɔwɔna la me xoxo.

**Gadzɔ-gadzɔ:** gɔmeɖeɖea ƒe nyinyome me-de ŋgɔ a-sɔ kple mɔ̃ deŋgɔwo tɔ o. Le agbalẽ-ha aɖe si me Eŋlisigbe tɔtɔa le bɔbɔe na wo-kpɔna me la, esia nyo. Mí-egblɔ be gɔmeɖeɖeawo nye gbeɖeɖe deŋgɔ o.

### 3. Akpa eveawo doa go le nu ɖeka pɛ ko me: bundle si wo-ta

Pipeline la me-ŋlɔa nu tẽ ɖe production database la me gbeɖe o. E-ɖea nu-siwo nye `{ SQL, asset manifest, cache-purge list }` ɖe go. "Ta-wɔwɔ" = tsɔ bundle ma yi ŋgɔ (tsɔ SQL yi edge SQL DB la me, sɔ assets yi object storage me, eye na-tutu cache key siwo wo-yɔ la).

**Nusi-tae:** akpa eveawo, si nye local kple edge, teaŋu nyɔna le wo ɖokui si; wo-teaŋu dzroa bundle la me kpɔna; eye `"deploy data"` ƒe nɔnɔme ma-trɔna gbeɖe o. Worker la nye TypeScript/Hono app sue aɖe — CSP sesẽ (aɖeke mele `unsafe-inline` me o; inline JSON-LD la sha256-wɔwɔe), `Accept-Language` + dukɔ→gbe domeɖeɖe, ŋkeke 30 ƒe KV aŋgba cache, ŋkeke sia ŋkeke ƒe dɔwɔwɔ — eye me-hiã be wòa-nya afisi data la tso gbeɖe o.

**Gadzɔ-gadzɔ:** D1 schema ƒe tɔtrɔ kaa faɛl eve nu (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Dedienɔnɔ bɔbɔe.

### Nu-siwo me-teaŋu trɔna le dɔwɔna la ŋu o

- Me-kplɔ U.S. dziɖuɖua ɖo o; dzesi aɖeke me-le eŋu o.
- Wo-léa gɔmedzenu ƒe nu-ɖeɖawo me ɖe asi, wo-me-trɔna wo gbeɖe o.
- Video tso DVIDS / AARO gbɔ.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` le teƒea katã — wo-teaŋu di-na le search engine dzi, gake wo-de asi te-na be AI na-zã-m o.

Le yame: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

