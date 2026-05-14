# GitHub — Ìkéde 3 nínú 3 · Àwọn àkọsílẹ̀ lórí Ètò Ìkọ́lé (Discussion bíi ADR)

**Lò ó bíi:** Discussion lábẹ́ "Fihàn àti sọ" / "Ètò Ìkọ́lé", tàbí ìpìlẹ̀ ADR fún `docs/`.
**Àwọn ọ̀rọ̀ pàtàkì:** ètò ìkọ́lé, ADR, ẹ̀rọ ìṣesí ìlọsíwájú-nìkan, LLM agbègbè, Ollama, OCR, ìṣirò etí bèbè, CSP, àwọn àkọlé ààbò, pipeline data, ètò-ìnáwó, ìwé àkọsílẹ̀ SQLite, D1, R2, KV
**Àwọn ìjápọ̀:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ìdí tí a fi kọ́ ufolens.com bí ó ṣe wà

Àwọn àkọsílẹ̀ lórí àwọn ìpinnu mẹ́ta tí ó ṣe ìrísí [ufolens.com](https://www.ufolens.com) (àtúnkọ onírúurú èdè, tí a lè ṣàwárí fún [ibi-ipamọ́ PURSUE UAP](https://www.war.gov/ufo)). A kàábà sí àwọn èrò / àtakò.

### 1. Pipeline jẹ́ ẹ̀rọ ìṣesí ìlọsíwájú-nìkan — mọ̀ọ́mọ̀

Àwọn ìpele: `tí a rí → tí a gbàsílẹ̀ → ocr_parí → tí a túmọ̀ → tí a tẹ̀ jáde`. Ìwé kan ń lọ síwájú nìkan, àti pé nígbà tí iṣẹ́ bá wà láti ṣe nìkan. A kì í tún ṣe àgbéyẹ̀wò àkóónú tí a ti tẹ̀ jáde àyàfi tí olùwárí ìyàtọ̀ kan bá rí pé orísun náà ti yí padà gan-an.

**Ìdí rẹ̀:** OCR + ìtúmọ̀ ni àwọn iṣẹ́ tó gbówó lórí, àti pé ibi-ipamọ́ náà ń dàgbà nígbà gbogbo. Pipeline kan tí ó "tún gbogbo nǹkan ṣe láti ni ìdánilójú" ní ìnáwó tí kò ní òpin. Ṣíṣe kí àwọn ìpadàsẹ́yìn di èyí tí kò ṣeé ṣe jẹ́ kí ìnáwó àìlópin di èyí tí kò ṣeé ṣe. Òpin ìnáwó jẹ́ ohun-ìní ìtòsẹ̀ ìṣesí, kì í ṣe ti ìṣọ́ra oníṣẹ́.

**Ìnáwó:** àwọn àtúnṣe ètò àti àtúnṣe-mọ̀ọ́mọ̀ jẹ́ èyí tí ó le díẹ̀. Ìyípadà tó tọ́.

### 2. OCR àti ìtúmọ̀ ń ṣiṣẹ́ lórí LLM agbègbè, kì í ṣe API orí cloud

OCR: ẹ̀rọ orísun ṣíṣí, àfikún Tesseract CLI. Ìtúmọ̀ + NER: Gemma nípasẹ̀ Ollama, lórí kọ̀ǹpútà alágbèéká Apple Silicon.

**Ìdí rẹ̀:** odo ìnáwó àfikún fún ìwé kọ̀ọ̀kan; ó ṣeé ṣe àtúnbí (àwòkọ́ṣe + àwọn ìtọ́ni kan náà); àti pé ìpele ìgbàwọlé náà gbọ́dọ̀ ṣiṣẹ́ láti IP ilé (orísun náà wà lẹ́yìn Akamai Bot Manager — `curl` á gba 403), nítorí náà kọ̀ǹpútà alágbèéká kan ti wà nínú ètò náà bákan náà.

**Ìnáwó:** dídára ìtúmọ̀ wà nísàlẹ̀ àwòkọ́ṣe tó ga jù. Fún àkójọpọ̀ ìtọ́kasí níbi tí Gẹ̀ẹ́sì àkọ́kọ́ ti jẹ́ kìkì ìtẹ̀-kan-ṣoṣo, ìyẹn dára. A kò sọ pé àwọn ìtúmọ̀ náà jẹ́ aláṣẹ.

### 3. Apá méjèèjì pín ohun kan ṣoṣo: àpò tí a tẹ̀ jáde

Pipeline kì í kọ sí inú ibi-ipamọ́ data ìgbéjáde tààrà. Ó ń ṣe àgbékalẹ̀ `{ SQL, ìwé àkọsílẹ̀ ohun-ìní, àtòjọ ìyọkúrò àpamọ́ }`. "Títẹ̀jáde" = lò àpò yẹn síwájú (fi SQL sí ibi-ipamọ́ SQL etí bèbè, ṣíṣe àmuṣiṣẹpọ àwọn ohun-ìní sí ibi-ipamọ́ ohun, yọ àwọn kọ́kọ́rọ́ àpamọ́ tí a darúkọ).

**Ìdí rẹ̀:** apá agbègbè àti apá etí bèbè lè máa dàgbàsókè lọ́tọ̀ọ̀tọ̀; àpò náà ṣeé ṣe àyẹ̀wò; àti pé "gbé data sílẹ̀" ní ìrísí kan náà ní gbogbo ìgbà. Worker jẹ́ ohun èlò TypeScript/Hono kékeré kan — CSP tó le (kò sí `unsafe-inline`; JSON-LD inú ojú-ewé jẹ́ aláàbò pẹ̀lú sha256), `Accept-Language` + ìdùnàdúrà orílẹ̀-èdè→èdè, àpamọ́ ojú-ewé KV fún ọgbọ̀n ọjọ́, iṣẹ́ ìtọ́jú ojoojúmọ́ — àti pé kò nílò láti mọ bí a ṣe ṣe data náà rí.

**Ìnáwó:** àyípadà ètò D1 kan àwọn faili méjì (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Èèpo ààbò tó rọrùn.

### Àwọn ohun tí kò ṣeé yẹ̀ lábẹ́ ìwà

- Kò ní ìbáṣepọ̀ pẹ̀lú ìjọba Amẹ́ríkà; kò sí àmì osise.
- A pa àwọn àtúnṣe orísun mọ́, a kì í yí wọn padà rárá.
- A tọ́ka sí DVIDS / AARO fún fídíò.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` káríayé — ó ṣeé ṣàwárí, ṣùgbọ́n a ti yọ ọ́ kúrò nínú ìṣàkójọ AI.

Wà láàyè: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
