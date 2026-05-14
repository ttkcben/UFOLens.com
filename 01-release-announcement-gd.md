# GitHub — Post 1 de 3 · Fios mu fhoillseachadh / bloc README

**Cleachd mar:** chorp GitHub Release, Deasbad prìneach, no aig mullach an README repo.
**Faclan-luirg:** UAP, UFO, tasglann PURSUE, sgrìobhainnean dì-chlasaichte, dàta fosgailte, làn-theacsa rannsachadh, OCR, eadar-theangachadh inneil, LLM ionadail, Ollama, coimpiutaireachd iomaill, API poblach, Hono, TypeScript, Python
**Ceanglaichean-lìn:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — àrd-ùrlar ioma-chànanach, so-rannsaichte airson tasglann PURSUE UAP

**Beò:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Tùs-tasglainn:** https://www.war.gov/ufo

Bidh `ufolens.com` ag ath-fhoillseachadh tasglann **PURSUE** Roinn Cogaidh nan S.A. de chlàran dì-chlasaichte UAP / UFO mar àrd-ùrlar eòlais: làn-theacsa rannsachadh, eadar-theangachadh inneil thar a’ chorpas, rannsachadh mapa + loidhne-tìm, agus API JSON poblach. Tha sgrìobhainnean tùsail nan obraichean aig riaghaltas feadarail nan S.A. agus taobh a-staigh nan S.A. tha iad san raon phoblach ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Chan eil am pròiseact seo **ceangailte ri riaghaltas nan S.A.**, chan eil e a’ cleachdadh suaicheantasan oifigeil, agus cha chuir e cùl ri deasachaidhean gu bràth.

### Ailtireachd

```
Inneal ionadail (Apple Silicon, IP còmhnaidheach)    Lìonra iomaill
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, cridhe stdlib-a-mhàin)      worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (air adhart a-mhàin)    /{lang}/...   duilleagan
  OCR: einnsean stòr-fosgailte (Tesseract CLI mar chùl-taic)  /api/v1/...   API poblach
  translate / NER: LLM ionadail (Gemma tro Ollama)      /admin        consol an gnìomhaiche
  stàit: foillseachadh SQLite                              le taic bho: stòr-dàta SQL iomaill, stòradh
        │                                              nithean (PDFan tùsail), tasgadan KV
        └── a’ foillseachadh pasgan: SQL + foillseachadh maoin + liosta glanadh tasgadan ──┘
```

- **Cosgais neoni gach sgrìobhainn san sgòth-AI.** Bidh OCR agus eadar-theangachadh a’ ruith gu h-ionadail; tha an inneal-stàite a ghluaiseas air adhart a-mhàin (`discovered → downloaded → ocr_done → translated → published`) a’ gealltainn nach tèid sgrìobhainn ath-phròiseasadh mura h-atharraich i.
- **Chan eil eisimeileachd treas-phàrtaidh aig cridhe na pìob-loidhne** — bidh modalan parsadh / foillseachadh / delta a’ ruith agus a’ dèanamh deuchainn air Python glan gun dad air a stàladh le pip; bidh ìrean OCR/eadar-theangachaidh ag ìsleachadh gu gràsmhor nuair a tha pacaidean roghainneil a dhìth.
- **Bidh an làrach iomaill** a’ cur an sàs bannan-cinn tèarainteachd teann + CSP (gun `unsafe-inline`; tha JSON-LD in-loidhne air a phrìneachadh le sha256), rèiteachadh cànain tro `Accept-Language` + mapadh dùthcha, tasgadan duilleag KV 30-latha, agus cron glanadh-taighe làitheil.
- **Ùrachaidhean mean air mhean:** bidh lorgaire delta a’ dèanamh coimeas eadar clàr-amais an tùs agus a’ biathadh dìreach atharraichean air ais dhan phìob-loidhne.

### Airson luchd-leasachaidh

Bidh an API poblach aig https://www.ufolens.com/api/v1 a’ tilleadh sgrìobhainnean agus meata-dàta mar JSON. Tha ruigsinneachd gun urra air a chuingealachadh a thaobh reata; iarr iuchair airson ìrean rannsachaidh/leasaiche. Faic an earrann API air an làrach airson cinn-uidhe agus crìochan.

### Inbhe

Còd coileanta; làrach air a sgaoileadh aig https://www.ufolens.com. Tha stòr-dàta an riochdachaidh air a lìonadh le bhith a’ ruith na pìob-loidhne far-loidhne agus a’ foillseachadh a’ phasgan air adhart (`cli_publish run --remote`). Tha sgrìobhainnean dealbhaidh slàn beò ann an `docs/20260511/`.

### Ceadachas / crìochan

- Sgrìobhainnean tùsail: Obraichean riaghaltas feadarail nan S.A., raon poblach taobh a-staigh nan S.A.
- Còd an àrd-ùrlair seo fhèin: faic `LICENSE`.
- Bidh an làrach a’ cur `Tdm-Reservation: 1` agus `X-Robots-Tag: noai, noimageai` — so-chlàr-amais le einnseanan luirg, air roghnachadh a-mach à trèanadh/sgrìobadh AI.
- Tha fiolm bhidio air a thoirt do DVIDS / AARO agus chan eil am pròiseact seo ga thagradh.

Thathas a’ cur fàilte air cùisean agus PRs. Feuch an leugh thu `CLAUDE.md` agus `docs/20260511/00-*` mus fosgail thu atharraichean structarail.
