# GitHub — Saƙo na 3 daga cikin 3 · Bayanan Tsarin Gine-gine (Tattaunawar salon ADR)

**Yi amfani da shi azaman:** Tattaunawa a ƙarƙashin "Nuna da Faɗa" / "Tsarin Gine-gine", ko tushen ADR na `docs/`.
**Mahimman kalmomi:** tsarin gine-gine, ADR, injin jiha mai tafiya-gaba-kawai, LLM na gida, Ollama, OCR, lissafin gefe, CSP, matakan tsaro, bututun bayanai, injiniyan kuɗi, bayanin SQLite, D1, R2, KV
**Masu haɗin gwiwa:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Dalilin da ya sa aka gina ufolens.com a wannan hanyar

Bayanai kan yanke shawara uku da suka tsara [ufolens.com](https://www.ufolens.com) (sake gina [taskar PURSUE UAP](https://www.war.gov/ufo) mai bincike da harsuna da yawa). Ana maraba da ra'ayoyi / suka.

### 1. Pipeline injin jiha ne mai tafiya-gaba-kawai — da gangan

Jihuna: `an gano → an sauke → an gama ocr → an fassara → an wallafa`. Takarda tana tafiya gaba ne kawai, kuma sai lokacin da akwai aikin da za a yi. Abubuwan da aka wallafa ba a sake sarrafa su sai dai idan mai gano delta ya ga asalin ya canza.

**Dalili:** OCR + fassara sune ayyuka masu tsada, kuma taskar tana girma a tsawon lokaci. Pipeline da ke "sake gudanar da komai don tabbatar da aminci" yana da kuɗin da ba shi da iyaka. Hana komawa baya yana hana samun kuɗin da ba a zata ba. Iyakar kuɗin sifa ce ta tsarin jiha, ba na taka-tsantsan na mai aiki ba.

**Kuɗi:** ƙaura daga tsari da sake sarrafawa da gangan suna da wahalar da aka yi niyya. Ciniki ne da aka yarda da shi.

### 2. Ana gudanar da OCR da fassara a kan LLM na gida, ba API na gajimare ba

OCR: injin budadden tushe, tare da Tesseract CLI a matsayin madadin. Fassara + NER: Gemma ta hanyar Ollama, a kan kwamfutar tafi-da-gidanka ta Apple Silicon.

**Dalili:** babu ƙarin kuɗi a kowace takarda; mai maimaitawa (samfuri da umarni tsayayyu); kuma matakin samo bayanai dole ne ya gudana daga adireshin IP na gida (asalin yana bayan Akamai Bot Manager — `curl` yana samun 403), don haka kwamfutar tafi-da-gidanka tana cikin aikin ko ta yaya.

**Kuɗi:** ingancin fassara bai kai na babban samfuri ba. Ga rumbun adana bayanai inda ainihin Turanci koyaushe yana kusa da dannawa ɗaya, wannan ya yi kyau. Ba mu yi iƙirarin cewa fassarorin ingantattu bane.

### 3. Sassan biyu suna raba ainihin mu'amala guda ɗaya: haɗin da aka wallafa

Pipeline ba ya rubuta kai tsaye zuwa rumbun adana bayanai na samarwa. Yana fitar da `{ SQL, bayanin kadarori, jerin goge ma'ajiya }`. "Wallafawa" = aiwatar da wannan haɗin gaba (tura SQL zuwa DB na SQL na edge, daidaita kadarori zuwa ma'ajiyar abu, goge maɓallan ma'ajiyar da aka ambata).

**Dalili:** bangaren gida da bangaren edge na iya haɓaka da kansu; ana iya duba haɗin; kuma "tura bayanai" yana da siffa iri ɗaya kowane lokaci. Worker karamin app ne na TypeScript/Hono — CSP mai tsauri (babu `unsafe-inline`; an maƙala JSON-LD na ciki da sha256), `Accept-Language` + tattaunawar ƙasa→harshe, ma'ajiyar shafi na KV na kwanaki 30, cron na tsaftacewa na yau da kullun — kuma baya buƙatar sanin yadda aka samar da bayanan.

**Kuɗi:** canjin tsarin D1 yana shafar fayiloli biyu (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Inshora mai arha.

### Abubuwan da ba za a iya sasantawa ba da aka gina a cikin halayya

- Ba shi da alaƙa da gwamnatin Amurka; babu alamun hukuma.
- An adana gyare-gyaren asali, ba a taɓa cire su ba.
- Bidiyo an danganta shi ga DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` a duk shafin — injunan bincike na iya gani, an fita daga kwafin AI.

Kai tsaye: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
