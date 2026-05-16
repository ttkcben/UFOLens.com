# GitHub — Fas 1 long 3 · Rilis / anonsmen blong README

**Yusum olsem:** wan GitHub Rilis bodi, wan stap olsem Diskason, o tab long README blong repozitori.
**Ol wod blong yusum:** UAP, UFO, PURSUE akaev, ol dokiumen we oli deklasifaem, open data, pul-teks sevis, OCR, masin transleisen, lokal LLM, Ollama, edge computing, pablik API, Hono, TypeScript, Python
**Ol link:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — wan platfom blong plante languis, we yu save lukluk long hem from PURSUE UAP akaev

**Laef olsem:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Sores akaev:** https://www.war.gov/ufo

`ufolens.com` hemi ri-pablisim akaev blong U.S. War Dipatmen we i stap long **PURSUE** blong ol deklasifaed UAP / UFO rikod olsem wan platfom blong save: pul-teks sevis, masin transleisen long olgeta dokiumen, map + taemlaen lukluk, mo wan pablik JSON API. Ol soresol dokiumen ol wok blong U.S. fedarel gavman mo insaed long U.S. ol i pablik domen ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Project ia **i no konekted wetem U.S. gavman**, i no yusum eni ofisel insignia, mo i neva reversim ol redaeksen.

### Akitekja

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

- **Nol kost long cloud-AI blong evri dokiumen.** OCR mo transleisen i ran lokal; ol forwod-oni stat masin (`discovered → downloaded → ocr_done → translated → published`) i garanitim se no dokiumen i save ri-proses from i no jenis.
- **Pipeline core i no gat eni tedi pati dipendensi** — ol parsing / manifes / delta mojiul i ran mo test long wan klia Python wetem no wan pip-installed; ol OCR/transleisen stejes i save wok gud taem ol opsenol pakij i no stap.
- **Edge saet** i aplaem ol strong sekiuriti hedaz + CSP (no `unsafe-inline`; inline JSON-LD sha256-pinned), languis negosieisen thru `Accept-Language` + kantri maping, wan 30-dei KV peij kas, mo wan deil hoz-kiping kron.
- **Ol niu apdet:** wan delta detekta i faenem ol jenis long soresol indeks mo i givim oljenis nomo bak long paeplaen.

### Blong ol divelopa

Pablik API long https://www.ufolens.com/api/v1 i ritanim ol dokiumen mo meta-data olsem JSON. Anonimus akses i gat limit blong hem; yu save rikwest wan ki blong risetsa/divelopa taez. Lukluk API seksen long saet blong ol endpoint mo ol limit.

### Steitus

Kod i pulap; saet i stap wok long https://www.ufolens.com. Ol data blong prodaksen i pulap taem yu ranim ol oflaen paeplaen mo yu pablisim ol bondol i go (cli_publish ran --rimot). Pulap dizain dokiumen i stap long `docs/20260511/`.

### Laisens / ol boder

- Ol soresol dokiumen: Ol wok blong U.S. fedarel gavman, pablik domen insaed long U.S.
- Ol kod blong platfom ia: lukluk `LICENSE`.
- Saet i sendem `Tdm-Reservation: 1` mo `X-Robots-Tag: noai, noimageai` — i save indeks long ol sevis enjin, i no tek pat long AI trening/skraping.
- Ol vidio futij i blong DVIDS / AARO mo project ia i no mekem klaem long hem.

Ol isu mo PR i welkam. Plis ridim `CLAUDE.md` mo `docs/20260511/00-*` bifo yu openim ol jenis long strakja.

