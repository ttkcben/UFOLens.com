# GitHub — Winndannde 1 nder 3 · Feccere yaltinnde / udditirde README

**Huutoraade no:** ɓanndu GitHub Release, Jeewte-jeewte pinnaaɗe, malla dow fuɗɗam repo README.
**Konnguɗi teeŋtuɗi:** UAP, UFO, defterdu PURSUE, binndi deestaaɗi, kabaruuji udditiiɗi, ɗaɓɓitorde binndol timmungol, OCR, firugol-masin, LLM nokkuure, Ollama, hisnaaki dow-ko-toɓɓe, API yimɓe fuu, Hono, TypeScript, Python
**Jokkorli:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — dingiral ɗemɗe keewɗe, ɗaɓɓitotoongal ngam defterdu PURSUE UAP

**To woodi:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Defterdu iwdi:** https://www.war.gov/ufo

`ufolens.com` ina yaltina kadi defterdu **PURSUE** Departemaa Hare Aameerik nde binndi UAP / UFO deestaaɗi bana dingiral anndal: ɗaɓɓitorde binndol timmungol, firugol-masin nder defterdu fuu, ɗaɓɓitorde karte + limngal-wakkati, e API JSON yimɓe fuu. Binndi iwdi ɗin ko golle laamu federaal Aameerik, nder Aameerik ɗe ngoni nder yimɓe fuu ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Porogaram o **hawtaaki e laamu Aameerik**, o huutortaako alaama laamu, o meeɗaa waylude ko suuɗaa.

### Mahdi

```
Masiŋa nokkuure (Apple Silicon, IP hoɗorde)          Reso dow-ko-toɓɓe
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, ɓanndu stdlib-only)         worker/  (TypeScript, Hono.js)
  heɓtude → OCR → firde → yaltinde (yahde-to-yeeso)    /{lang}/...   kelle
  OCR: masiŋa udditiiɗo (Tesseract CLI walla)          /api/v1/...   API yimɓe fuu
  firde / NER: LLM nokkuure (Gemma rewrude e Ollama)   /admin        konngol gollinoowo
  statu: deftere SQLite                              ballitiraama e: DB SQL dow-ko-toɓɓe,
        │                                              resrude (PDFs iwdi), cache KV
        └── ina yaltina go'o: SQL + deftere jawdi + limngal ittude-cache ──┘
```

- **Soodgol AI-cloud gooto-gooto ngam kala binndol.** OCR e firugol ina golla nder nokkuure; masiŋa statu yahde-to-yeeso (`yiitaama → jippinaama → ocr_timmi → firtaama → yaltinaama`) ina tabitina binndol ngol waylataako so wonaa si wayliima.
- **Ɓanndu pipeline walaa jokkorli tataɓi** — modules parsing / manifest / delta ina golla e jarriboo dow Python laaɓɗo mo alaa ko pip-installed; stages OCR/firugol ina ustoo no haanirta so packages optional walaa.
- **Lowre dow-ko-toɓɓe** ina huutora headers kisal tiiɗɗi + CSP (alaa `unsafe-inline`; inline JSON-LD sha256-pinned), yeewtere ɗemngal rewrude e `Accept-Language` + mapgol leydi, cache kelle KV balɗe 30, e cron laɓɓinirɗo kala nyalawma.
- **Waylitaare ɓeydotoonde:** yiytorde delta ina seerta index iwdi e ina hokka tan waylitaare nder pipeline.

### Ngam ɓamtooɓe

API yimɓe fuu to https://www.ufolens.com/api/v1 ina hokka binndi e metadata bana JSON. Naatgol anonim ina jogii keerol; naamno coktirgal ngam levels ɗaɓɓitoowo/ɓamtoowo. Ndaaree feccere API dow lowre ngam anndude endpoints e keeritte.

### Statu

Kod timmi; lowre yaltinaama to https://www.ufolens.com. Database production ina hebbinee e gollinde pipeline offline e yaltinde go'o yahde-to-yeeso (`cli_publish run --remote`). Binndi design timmuɗi ina ngoodi nder `docs/20260511/`.

### Lisansi / Keeritte

- Binndi iwdi: golle laamu federaal Aameerik, yimɓe fuu nder Aameerik.
- Kod dingiral ngal e hoore mum: ndaaree `LICENSE`.
- Lowre ndee ina nelda `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — ina waawi wonde index e masiŋaaji ɗaɓɓitorde, woppii janngingol/scraping AI.
- Wideyooji ina njeyaa e DVIDS / AARO, porogaram o wi'aani ko kamɓe njeyi ɗi.

Caɗeele e PRs ina njaɓɓaama. Tiiɗnaaree janngude `CLAUDE.md` e `docs/20260511/00-*` hade mon udditde waylitaare mawnde.

