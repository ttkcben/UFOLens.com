# GitHub — Aselqan 1 deg 3 · Tabert n tufɣa / Talɣa n ubuyaz README

**Seqdec s:** Yiwen ubeḥri n tufɣa n GitHub, yiwen usegzi iṛeẓmen, neɣ aqerru n README n umyag.
**Awalen ileqqmen:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Isemtuyen:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — taɣult s waṭas n tutlayin, yezmer ad yettunadi, i umazzlu n PURSUE UAP

**Tizert:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Amazzlu aẓṛi:** https://www.war.gov/ufo

`ufolens.com` tesɛaḍ ɣer ssebbat azzlu n **PURSUE** n Weqqas n Tussna n Weɛraq n Marikan, ayen yeǧǧan isefka n UAP / UFO d wid yeffken ɣer uwanak, d taɣult n tussna: unadi amagnu, aseqdec n tutlayt s tmunt ɣef umussu, tazrawt n lexrit n wakud, akked yiwen n API n JSON i umussu. Isefka iẓṛiyan d lecɣal n tḥukumt tafederalt n Marikan, u deg Marikan d akken-iten d aɣawan ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Aheggan-agi **ur yesɛi ara aṭṭaf d tḥukumt n Marikan**, ur yesseqdec ara isem n uwanak, yerna ur yezmir ara ad yessuɣul asefru.

### Amecwar

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

- **Ur yelli ara ugur n tulay n cloud-AI i yal isefru.** OCR d usegzi ttnernint s wudem aẓṛi; aseqdec n umazzlu n isefka yettwalasen (`discovered → downloaded → ocr_done → translated → published`) yezmer ad yefk abrid n usegzi n isefka i tnernit.
- **Pipeline core ur yesɛi ara aṭṭaf d tɣawsiwin nniḍen** — parsing / manifest / delta modules ttnernint d ttemḍiɣent ɣef yiwen n Python yeldin s tuẓẓum n pip-installed; OCR/translation stages tteɣlin s wudem yelhan m’ur telli ara tɣawsa n iḥekmen.
- **Edge site** tesɛaḥḥar isemẓa n tɣellist iǧehden + CSP (ur yelli ara `unsafe-inline`; JSON-LD sha256-pinned), ameslay n tutlayt s `Accept-Language` + lexrit n tmurt, KV page cache n 30 n wussan, akked yiwen n cron n uḥerrek n yal ass.
- **Ibeddel n tnernit:** yiwen n delta detector yettwaɣay s isemẓa n umazzlu, yerna yettwaḍfar s ibeddel kan ɣer pipeline.

### Iɣellafen

API umussu deg https://www.ufolens.com/api/v1 yessuɣul isefka d isefka n umussu s JSON. Aseqdec n anonyme yettwaḥṣa s uḥbas n tnernit; seklec isemẓa n uḥbas i tnernit n imesnulfuyen/iɣellafen. Wal assaɣ n API deg usmel i imeslayen d iḥebsiwen.

### Aḥeqran

Amasay amagnu; usmel yettwaɛeddel deg https://www.ufolens.com. Amazzlu n isefka n tnernit yettwaɛeddel s usegzi n pipeline n ufay, yerna yettwaɛeddel s usegzi n bundle (`cli_publish run --remote`). Isefka n umecwar amagnu ddant deg `docs/20260511/`.

### Turagt / iḥebsiwen

- Isefka iẓṛiyan: Lecɣal n tḥukumt tafederalt n Marikan, aɣawan deg Marikan.
- Amasay n platform-agi: wal `LICENSE`.
- Asmel yessuɣul `Tdm-Reservation: 1` d `X-Robots-Tag: noai, noimageai` — yezmer ad yettwaɣay s iḍrisen n unadi, ur yettwaɣay ara s AI training/scraping.
- Tizi n vidyu yettwaɛeddel ɣer DVIDS / AARO, yerna ur yettwaɛeddel ara s aheggan-agi.

Iɣeblan d PRs ssexdamen. Ad tɣeṛṛeḍ `CLAUDE.md` d `docs/20260511/00-*` uqbel ad teldid ibeddel imagnuyen.
