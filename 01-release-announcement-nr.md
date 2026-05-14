# GitHub — Indatshana 1 kwezi-3 · Isikhumulo / Isibhengezo se-README

**Sebenzisa njenge-:** Umzimba wokukhululwa kwe-GitHub, Ingxoxo ebethelwe, namkha phezulu kwe-README ye-repo.
**Amagamaqangi:** UAP, UFO, PURSUE archive, amaphepha aveziweko, idatha evulekileko, ukutlola koke, OCR, ukuhumusha ngomshini, i-LLM yangekhaya, i-Ollama, i-edge computing, i-API yomphakathi, i-Hono, i-TypeScript, i-Python
**Izixhumanisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — isibaya esineelimi ezinengi, esingahlolisiseka se-PURSUE UAP archive

**Bukhoma:** https://www.ufolens.com · **I-API:** https://www.ufolens.com/api/v1 · **I-archive yomthombo:** https://www.war.gov/ufo

`ufolens.com` iphinda ikhuphe i-archive ye-PURSUE yomNyango weZemphi wase-U.S. yamarekhodi we-UAP / UFO aveziweko njengesibaya solwazi: ukutlola koke, ukuhumusha ngomshini kuwo woke umtapo, ukuhlolwa kwemephu + ilayini yesikhathi, kunye ne-API ye-JSON yomphakathi. Amaphepha womthombo yimisebenzi karhulumende we-U.S. begodu ngaphakathi kwe-U.S. asesidlangalaleni ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Iprojekthi le **ayihlangani norhulumende we-U.S.**, ayisebenzisi iimbotjhisi ezisemthethweni, begodu ayinqabuluki ukungenelela.

### Imithethokhandla

```
Local machine (Apple Silicon, residential IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Ayikho imali ye-cloud-AI ngedokhumente ngayinye.** I-OCR nokuhumusha kwenzeka endaweni; umshini we-state oya phambili kwaphela (`otholiwe → olandiwe → i-ocr yenziwe → ihunyushiwe → iphatlaladisiwe`) iqinisekisa bona akukho dokhumente ephinda ilungiswe ngaphandle kobana yatjhuguluka.
- **I-pipeline core ayinawo ama-dependency wangaphandle** — ama-module wokuhlola / i-manifest / i-delta asebenza begodu ahlolwa ku-Python ehlanzekileko ngaphandle kwe-pip efakiweko; izigaba ze-OCR/ukuhumusha ziyancipha ngokuthoma nangabe ama-package wokuzikhethela angekho.
- **I-edge site** isebenzisa ama-header wokuphepha aqinileko + i-CSP (akukho `unsafe-inline`; i-JSON-LD yangekhaya ishaywa nge-sha256), ukukhulumisana ngeelimi nge-`Accept-Language` + ukumapa kwelizwe, i-cache ye-KV yelikhasi yamasuku ama-30, kunye ne-cron yokuhlanza yansuku zoke.
- **Iintjhijilelo ezingejako:** i-delta detector ihlola i-source index begodu ifaka iintjhijilelo kwaphela emuva ku-pipeline.

### Kubathuthukisi

I-API yomphakathi ku-https://www.ufolens.com/api/v1 ibuyisa amaphepha kunye ne-metadata njenge-JSON. Ukufinyelela okungaziwa kubekelwe imikhawulo ngesivinini; cela isitjhukutjhelo seengaba zabatloli/abathuthukisi. Bona isigaba se-API esitendeni mayelana nama-endpoint kunye nemikhawulo.

### Isimo

Ikhowudi iphelile; isiteji sibekwe ku-https://www.ufolens.com. I-database yokukhiqiza igcwaliswa ngokusebenzisa i-offline pipeline begodu iphatlaladisa i-bundle phambili (`cli_publish run --remote`). Amaphepha wokuklama apheleleko atholakala ku-`docs/20260511/`.

### Ilayisensi / imikhawulo

- Amaphepha womthombo: imisebenzi karhulumende we-U.S., esidlangalaleni ngaphakathi kwe-U.S.
- Ikhowudi yesibaya lesi: bona i-`LICENSE`.
- Isiteji sithumela i-`Tdm-Reservation: 1` kunye ne-`X-Robots-Tag: noai, noimageai` — siyahlolisiseka ngeenjini zokuhlola, asifakiwe ekufundiseni/ukukhiqizeni nge-AI.
- Ividiyo itjho bona ivela ku-DVIDS / AARO begodu ayitjhoko bona yale projekthi.

Imiba nama-PR ayamukelekile. Sicela ufunde i-`CLAUDE.md` kunye ne-`docs/20260511/00-*` ngaphambi kokwenza iintjhijilelo ezingokwakha.

