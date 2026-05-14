# GitHub — Post 1 of 3 · Liyebisi ya Release / bloc ya README

**Salelá lokola:** nzoto ya GitHub Release, Discussion oyo epikami, to likoló ya README ya repo.
**Maloba ya ntina:** UAP, UFO, PURSUE archive, mikanda ya declassified, open data, bolukiluki ya maloba nyonso, OCR, libongoli ya machine, local LLM, Ollama, edge computing, API ya bato nyonso, Hono, TypeScript, Python
**Ba hyperliens:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — ebombelo ya minoko mingi mpe ya bolukiluki mpo na archive ya PURSUE UAP

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archive ya source:** https://www.war.gov/ufo

`ufolens.com` ezali kozongisa libimisi ya archive **PURSUE** ya Departema ya Etumba ya Amerika ya mikanda ya UAP / UFO oyo esili kofungolama lokola ebombelo ya boyebi: bolukiluki ya maloba nyonso, libongoli ya machine na kati ya corpus mobimba, botali ya karte + molongo ya ntango, mpe API ya JSON ya bato nyonso. Mikanda ya source ezali misala ya leta ya Amerika mpe na kati ya Amerika ezali na ngambo ya bato nyonso ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Mosala oyo **ezali na boyokani na leta ya Amerika te**, esaleli bilembo ya leta te, mpe ezongisaka ata mokolo moko te makambo oyo elongolami.

### Botongi

```
Machine locale (Apple Silicon, IP ya ndako)          Réseau ya pembeni
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   nkasa
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   API ya bato nyonso
  translate / NER: local LLM (Gemma via Ollama)        /admin        console ya mosali
  state: SQLite manifest                             esimbami na: edge SQL DB, esika ya
        │                                              biloko (ba PDF ya source), cache ya KV
        └── ebimisi liboke: SQL + manifest ya biloko + liste ya cache-purge ──┘
```

- **Mosolo ya zero mpo na AI ya cloud mpo na mokanda moko na moko.** OCR mpe libongoli esalemaka na esika moko; machine ya état oyo ekendeke kaka liboso (`discovered → downloaded → ocr_done → translated → published`) endimisi ete mokanda moko te ekozongelama longola se soki ebongwani.
- **Motema ya pipeline ezali na ba dépendances ya bato misusu te** — ba modules ya parsing / manifest / delta ekendeke mpe emekamaka na Python ya peto kozanga eloko ya pip; biteni ya OCR/libongoli esalaka malamu ata soki ba paquets oyo ezali obligatoire te ezangi.
- **Site ya pembeni** etie ba-headers ya sécurité ya makasi + CSP (kozanga `unsafe-inline`; inline JSON-LD sha256-pinned), botali ya monoko na nzela ya `Accept-Language` + boyokani ya mboka, cache ya lokasa ya KV ya mikolo 30, mpe cron ya misala ya mokolo na mokolo.
- **Mises à jour ya mokemoke:** eloko moko oyo emonaka mbongwana etalaka index ya source mpe ezongisaka kaka mbongwana na kati ya pipeline.

### Mpo na ba développeurs

API ya bato nyonso na https://www.ufolens.com/api/v1 ezongisaka mikanda mpe metadata lokola JSON. Accès anonyme ezali na ndelo; sengá fungola mpo na ba niveaux ya bolukiluki/développeur. Tala eteni ya API na site mpo na ba endpoints mpe bandelo.

### Etat

Code esili; site etiami na https://www.ufolens.com. Base de données ya production etondisami na kosalela pipeline ya offline mpe kobimisa liboke liboso (`cli_publish run --remote`). Mikanda nyonso ya conception ezali na `docs/20260511/`.

### Lisence / Bandelo

- Mikanda ya source: misala ya leta ya Amerika, domaine public na kati ya U.S.
- Code ya ebombelo oyo moko: tala `LICENSE`.
- Site etindaka `Tdm-Reservation: 1` mpe `X-Robots-Tag: noai, noimageai` — ekoki kozwama na ba moteurs de recherche, epekisami na AI training/scraping.
- Bilili ya video ezali ya DVIDS / AARO mpe mosala oyo emilobeli yango te.

Ba issues mpe ba PRs biyambi. Tosengi yo otanga `CLAUDE.md` mpe `docs/20260511/00-*` liboso ya kofungola mbongwana ya monene.

