# GitHub — Post 1 of 3 · Isaziso sokukhishwa / i-README block yesimemezelo

**Sebenzisa njenge:** umzimba we-GitHub Release, i-Discussion eshicilelwe, kumbe phezulu kwe-repo README.
**Amagama angukhiye:** UAP, UFO, PURSUE archive, amadokhumende akhishwe esidlangalaleni, idatha evulekile, ukusesha okugcwele, OCR, ukuhumusha ngomshini, i-local LLM, Ollama, i-edge computing, i-public API, Hono, TypeScript, Python
**Izixhumanisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — inkundla yezilimi eziningi, engasesheka ye-PURSUE UAP archive

**Ibukhoma:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Umlando womthombo:** https://www.war.gov/ufo

`ufolens.com` iphinda ishicilele i-U.S. War Department's **PURSUE** yamarekhodi e-UAP / UFO akhishwe esidlangalaleni njengepulatifomu yolwazi: ukusesha okugcwele, ukuhumusha ngomshini kuwo wonke umbhalo, imephu + ukuhlola isikhathi, kanye ne-public JSON API. Amadokhumende omthombo angumsebenzi kahulumeni wase-U.S. futhi ngaphakathi kwe-U.S. angumphakathi ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Le phrojekthi **ayihlangene nohulumeni wase-U.S.**, ayisebenzisi uphawu olusemthethweni, futhi ayikaze ihlehlise ukuhlelwa.

### I-Architecture

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

- **Izindleko ze-cloud-AI eziyindilinga ngedokhumende ngayinye.** I-OCR nokuhumusha kwenziwa endaweni; umshini we-state oya phambili kuphela (`discovered → downloaded → ocr_done → translated → published`) uqinisekisa ukuthi akukho dokhumende ephinda icutshungulwe ngaphandle uma ishintshile.
- **I-pipeline core ayinakho ukuncika kwezinkampani zangaphandle** — ukuhlukanisa / ukubonisa / amamojula e-delta asebenza futhi ahlolwe nge-Python ehlanzekile ngaphandle kwe-pip-installed; izigaba ze-OCR/translation zehlisa izinga ngobuhle lapho amaphakheji angakhethwa engekho.
- **I-Edge site** isebenzisa izinhloko zokuphepha eziqinile + CSP (akukho `unsafe-inline`; i-inline JSON-LD sha256-pinned), ukuxoxisana kolimi nge-`Accept-Language` + ukufanisa izwe, i-cache yepheji ye-KV yezinsuku ezingu-30, kanye ne-cron yokuhlanza yansuku zonke.
- **Ukuvuselelwa okunyukayo:** i-delta detector ihlukanisa inkomba yomthombo bese ifaka kuphela izinguquko emuva ku-pipeline.

### Kwabathuthukisi

I-public API ku-https://www.ufolens.com/api/v1 ibuyisela amadokhumende kanye nemethadatha njenge-JSON. Ukufinyelela okungaziwa kunemikhawulo yezinga; cela ukhiye wezinyathelo zocwaningo/abathuthukisi. Bheka isigaba se-API kusayithi ngamaphuzu okuphela kanye nemikhawulo.

### Isimo

Ikhodi igcwele; isayithi lifakwe ku-https://www.ufolens.com. I-database yokukhiqiza igcwaliswa ngokusebenzisa i-offline pipeline futhi ishicilele i-bundle phambili (`cli_publish run --remote`). Amadokhumende okuklama agcwele ahlala ku-`docs/20260511/`.

### Ilayisense / imikhawulo

- Amadokhumende omthombo: Imisebenzi kahulumeni wase-U.S., isidlangalala ngaphakathi kwe-U.S.
- Ikhodi yale nkundla: bheka `LICENSE`.
- Isayithi lithumela `Tdm-Reservation: 1` kanye `X-Robots-Tag: noai, noimageai` — liyasesheka ngezinjini zokusesha, likhishiwe ekuqeqeshweni kwe-AI/scraping.
- Ividiyo inikezwa ku-DVIDS / AARO futhi ayifunwa yile phrojekthi.

Izinkinga nama-PR zamukelekile. Sicela ufunde `CLAUDE.md` kanye `docs/20260511/00-*` ngaphambi kokuvula izinguquko zesakhiwo.

