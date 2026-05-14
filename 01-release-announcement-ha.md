# GitHub — Saƙo na 1 daga cikin 3 · Sanarwar Saki / README

**Yi amfani da shi azaman:** jikin Sanarwar Saki na GitHub, Tattaunawa da aka maƙala, ko a saman README na ma'ajiyar.
**Mahimman kalmomi:** UAP, UFO, taskar PURSUE, takardun da aka bayyana, buɗaɗɗen bayanai, cikakken binciken rubutu, OCR, fassarar inji, LLM na gida, Ollama, lissafin gefe, API na jama'a, Hono, TypeScript, Python
**Masu haɗin gwiwa:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — wani dandali mai harsuna da yawa, mai bincike don taskar PURSUE UAP

**Kai tsaye:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Taskar asali:** https://www.war.gov/ufo

`ufolens.com` yana sake wallafa taskar **PURSUE** ta Ma'aikatar Yaƙi ta Amurka wacce ke ɗauke da bayanan UAP / UFO da aka bayyana a matsayin dandali na ilimi: cikakken binciken rubutu, fassarar inji a cikin dukkanin rubuce-rubucen, binciken taswira + jerin lokaci, da kuma API na JSON na jama'a. Takardun asali ayyukan gwamnatin tarayyar Amurka ne kuma a cikin Amurka suna cikin mallakar jama'a ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Wannan aikin **ba shi da alaƙa da gwamnatin Amurka**, ba ya amfani da wata alama ta hukuma, kuma baya taba cire gyare-gyaren da aka yi.

### Tsarin Gine-gine

```
Na'ura ta gida (Apple Silicon, adireshin IP na gida)   Cibiyar sadarwa ta Edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, ainihin stdlib-kawai)      worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (tafiya-gaba-kawai) /{lang}/...   shafuka
  OCR: injin buɗaɗɗen tushe (Tesseract CLI madadin)   /api/v1/...   API na jama'a
  translate / NER: LLM na gida (Gemma ta Ollama)       /admin        wurin sarrafa ayyuka
  jiha: bayanin SQLite                                goyon bayan: DB na SQL na edge, ma'ajiyar
        │                                              abu (PDFs na asali), ma'ajiyar KV
        └── yana wallafa haɗin gwiwa: SQL + bayanin kadarori + jerin goge ma'ajiya ──┘
```

- **Babu kuɗin AI na gajimare a kan kowane takarda.** Ana gudanar da OCR da fassara a gida; injin jiha mai tafiya-gaba-kawai (`an gano → an sauke → an gama ocr → an fassara → an wallafa`) yana tabbatar da cewa ba za a sake sarrafa wata takarda ba sai dai idan ta canza.
- **Tsarin Pipeline na ainihi ba shi da dogaro kan wasu kamfanoni na waje** — sassan fassararwa / bayani / delta suna aiki kuma ana gwada su a kan Python mai tsabta ba tare da an shigar da komai ta pip ba; matakan OCR/fassara suna raguwa cikin sauƙi lokacin da fakitin zaɓi ba su nan.
- **Shafin Edge** yana amfani da tsauraran matakan tsaro na headers + CSP (babu `unsafe-inline`; an maƙala JSON-LD na ciki da sha256), tattaunawar harshe ta hanyar `Accept-Language` + taswirar ƙasa, ma'ajiyar shafi na KV na kwanaki 30, da kuma cron na tsaftacewa na yau da kullun.
- **Sabuntawa na mataki-mataki:** mai gano delta yana duba bambance-bambancen jeri na asali kuma yana mayar da canje-canje kawai cikin pipeline.

### Ga masu haɓakawa

API na jama'a a https://www.ufolens.com/api/v1 yana dawo da takardu da metadata a matsayin JSON. An iyakance damar shiga ba tare da suna ba; nemi mabuɗi don matakan masu bincike/masu haɓakawa. Duba sashin API a kan shafin don wuraren ƙarshe da iyakoki.

### Hali

An kammala rubuta lambar; an tura shafin a https://www.ufolens.com. Ana cika rumbun adana bayanai na samarwa ta hanyar gudanar da pipeline na layi da kuma wallafa haɗin gaba (`cli_publish run --remote`). Cikakken takardun zane suna cikin `docs/20260511/`.

### Lasisi / Iyakoki

- Takardun asali: Ayyukan gwamnatin tarayyar Amurka, mallakar jama'a a cikin Amurka.
- Lambar wannan dandamali: duba `LICENSE`.
- Shafin yana aika `Tdm-Reservation: 1` da `X-Robots-Tag: noai, noimageai` — injunan bincike za su iya gani, amma an fita daga horar da AI/kwafin bayanai.
- An danganta hotunan bidiyo ga DVIDS / AARO kuma wannan aikin bai mallake su ba.

Ana maraba da batutuwa da PRs. Da fatan za a karanta `CLAUDE.md` da `docs/20260511/00-*` kafin buɗe canje-canje na tsari.
