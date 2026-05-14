# GitHub — Nyatakaka 1 le 3 me · Gbeƒãɖeɖe / README ƒe nyatakaka

**Zã abe:** GitHub Gbeƒãɖeɖe ƒe agbalẽ, Nyamedzro si woƒo ka, alo README ƒe tame.
**Nya veviwo:** UAP, UFO, PURSUE archive, agbalẽ siwo ƒe adzame woɖe, data si ŋu amesiame ate ŋu akpɔ, ŋɔŋlɔ blibo didi, OCR, mɔ̃ɖegɔmeɖeɖe, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Kadodo veviwo:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — gbe-ge-ɖewo me nyagbɔgblɔ̃mɔ nuɖoanyi si me woa-teŋu a-di nu le na PURSUE UAP ƒe agbalẽnyadzɔdzɔ

**Le yame:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Agbalẽnyadzɔdzɔ tɔtɔ:** https://www.war.gov/ufo

`ufolens.com` gbugbɔ U.S. Aʋawɔnyawo Gbɔkpɔƒe ƒe **PURSUE** archive si me UAP / UFO ŋuti nya siwo ƒe adzame woɖe la tae abe nyanya ƒe nuɖoanyi ene: ŋɔŋlɔ blibo didi, mɔ̃ɖegɔmeɖeɖe le agbalẽ bliboa me, anyigbatata kple ɣeyiɣi ƒe ɖoɖo dzodzro, kple JSON API na amesiame. Agbalẽ tɔtɔawo nye U.S. ƒe dziɖuɖu blibo ƒe dɔwɔwɔwo eye le U.S. me la, amesiame ƒe nu wonye ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dɔwɔna sia **me-kplɔ U.S. dziɖuɖua ɖo o**, me-zãa dzesi aɖeke o, eye me-trɔa nu-siwo wo-ɖeɖa ɖa le eme gbeɖe o.

### Nusɔsrɔ̃ ƒe Ðoɖo

```
Mɔ̃ si le aƒeme (Apple Silicon, aƒeme IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Ao-xe naneke na alilikpo-AI ɖe agbalẽ ɖesiaɖe ta o.** OCR kple gɔmeɖeɖe dɔwɔnawo zɔna le mɔ̃ dzi le aƒeme; ŋgɔ-yiyi-ɖeɖe ƒe nɔnɔme mɔ̃ (`discovered → downloaded → ocr_done → translated → published`) la kpɔa egbɔ be wo-me-gbugbɔa agbalẽ aɖeke ƒe dɔ wɔ ge o negbe ɖe e-trɔ ko.
- **Pipeline ƒe dɔwɔnu vevitɔ me-hia ame evelia ƒe kpekpeɖeŋu aɖeke o** — parsing / manifest / delta dɔwɔnuwo zɔna eye wo-dona kpɔna le Python si me naneke me-le o dzi; OCR/gɔmeɖeɖe dɔwɔnawo wɔa dɔ nyuie ne nu-siwo hiã la me-le eme o hã.
- **Edge teƒe** zãa dedienɔnɔ ƒe sedede sesẽwo + CSP (aɖeke mele eme na `unsafe-inline` o; inline JSON-LD la sha256-wɔwɔe), gbe kple gbe domeɖeɖe to `Accept-Language` kple dukɔwo ƒe didi me, ŋkeke 30 ƒe KV aŋgba cache, kple ŋkeke sia ŋkeke ƒe dɔwɔwɔ.
- **Gbegbɔgblɔ̃ yeyewo siwo kpena ɖe eŋu vivivi:** delta detector aɖe kpɔa vovototo si le gɔmedzenu la me eye e-tsɔa nu yeyeawo ɖeɖe gbugbɔna dea pipeline la me.

### Na nu-ŋlɔ̃lawo

JSON API si le https://www.ufolens.com/api/v1 la tsɔa agbalẽwo kple woƒe metadatawo gbugbɔna vɛ le JSON me. Ame-siwo ƒe ŋkɔ me-nyo o la zazã li, gake wo-ɖo seɖoƒe na; bia be wo-na wò key na numekulawo/nu-ŋlɔ̃lawo ƒe zazã. Kpɔ API ƒe akpa si le teƒea ɖa na dɔwɔƒewo kple seɖoƒewo.

### Nɔnɔme

Wo-ŋlɔ code la vɔ; wo-da website la ɖi le https://www.ufolens.com. Wo-tsɔa offline pipeline la zazã kple bundle la gbugbɔ tae (`cli_publish run --remote`) tsɔ yɔa production database la me. Nusɔsrɔ̃ bliboa le `docs/20260511/` me.

### License / Seɖoƒewo

- Agbalẽ tɔtɔwo: U.S. ƒe dziɖuɖu blibo ƒe dɔwɔwɔwo, amesiame ƒe nu le U.S. me.
- Nuɖoanyi sia ƒe code tɔtɔ: kpɔ `LICENSE`.
- Teƒea ɖoa `Tdm-Reservation: 1` kple `X-Robots-Tag: noai, noimageai` ɖa — search engine-wo teaŋu di-na le eme, gake wo-de asi te-na be AI na-zã-m na hehe-xɔxɔ/nu-fifi o.
- Video siwo le eme la tso DVIDS / AARO gbɔ eye dɔwɔna sia me-xɔ ŋkɔ na o.

Mí-kpɔa dzidzɔ ɖe kuxíwo kple PRs ŋu. Meɖe kuku, xlẽ `CLAUDE.md` kple `docs/20260511/00-*` hafi na-ʋu tɔtrɔ gãwo.

