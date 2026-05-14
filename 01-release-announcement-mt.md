# GitHub — Post 1 minn 3 · Avviż ta' Rilaxx / Blokk ta' avviż README

**Użu bħala:** korp ta' Rilaxx ta' GitHub, Diskussjoni ffissata, jew fil-quċċata tar-README tar-repożitorju.
**Kliem ewlieni:** UAP, UFO, arkivju PURSUE, dokumenti deklassifikati, dejta miftuħa, tfittxija ta' test sħiħ, OCR, traduzzjoni awtomatika, LLM lokali, Ollama, edge computing, API pubblika, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — pjattaforma multilingwi u li tista' titfittex għall-arkivju PURSUE UAP

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Sors tal-arkivju:** https://www.war.gov/ufo

`ufolens.com` jippubblika mill-ġdid l-arkivju **PURSUE** tad-Dipartiment tal-Gwerra tal-Istati Uniti ta' rekords deklassifikati ta' UAP / UFO bħala pjattaforma ta' għarfien: tfittxija ta' test sħiħ, traduzzjoni awtomatika fil-corpus kollu, esplorazzjoni ta' mappa + linja tal-ħin, u API pubblika JSON. Id-dokumenti tas-sors huma xogħlijiet tal-gvern federali tal-Istati Uniti u fl-Istati Uniti huma fid-dominju pubbliku ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dan il-proġett **mhuwiex affiljat mal-gvern tal-Istati Uniti**, ma juża l-ebda emblema uffiċjali, u qatt ma jreġġa' lura r-redazzjonijiet.

### Arkitettura

```
Magna lokali (Apple Silicon, IP residenzjali)      Netwerk edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, qalba stdlib-only)        worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   paġni
  OCR: magna open-source (fallback Tesseract CLI)      /api/v1/...   API pubblika
  translate / NER: LLM lokali (Gemma permezz ta' Ollama) /admin        console tal-operatur
  stat: manifest SQLite                              appoġġjat minn: edge SQL DB, object
        │                                              storage (PDFs tas-sors), KV cache
        └── jippubblika pakkett: SQL + manifest tal-assi + lista ta' tindif tal-cache ──┘
```

- **Spiża żero għal kull dokument ta' cloud-AI.** L-OCR u t-traduzzjoni jaħdmu lokalment; il-magna tal-istat li timxi 'l quddiem biss (`discovered → downloaded → ocr_done → translated → published`) tiggarantixxi li l-ebda dokument ma jiġi pproċessat mill-ġdid sakemm ma jkunx inbidel.
- **Il-qalba tal-pipeline m'għandha l-ebda dipendenza fuq terzi partijiet** — il-moduli tal-ipproċessar / manifest / delta jaħdmu u jiġu ttestjati fuq Python nadif mingħajr xejn installat b'pip; l-istadji tal-OCR/traduzzjoni jiddegradaw b'mod grazzjuż meta l-pakketti mhux obbligatorji jkunu neqsin.
- **Is-sit edge** japplika headers tas-sigurtà stretti + CSP (l-ebda `unsafe-inline`; JSON-LD inline hu sha256-pinned), negozjar tal-lingwa permezz ta' `Accept-Language` + mapping tal-pajjiż, cache tal-paġna KV ta' 30 jum, u cron ta' manutenzjoni ta' kuljum.
- **Aġġornamenti inkrementali:** ditekter tad-delta jqabbel l-indiċi tas-sors u jdaħħal biss il-bidliet lura fil-pipeline.

### Għall-iżviluppaturi

L-API pubblika f'https://www.ufolens.com/api/v1 tirritorna dokumenti u metadata bħala JSON. L-aċċess anonimu huwa limitat fir-rata; itlob ċavetta għal-livelli ta' riċerkatur/iżviluppatur. Ara t-taqsima tal-API fuq is-sit għall-endpoints u l-limiti.

### Status

Kodiċi komplut; sit skjerat f'https://www.ufolens.com. Id-database tal-produzzjoni timtela billi titħaddem il-pipeline offline u jiġi ppubblikat il-pakkett 'il quddiem (`cli_publish run --remote`). Id-dokumenti tad-disinn sħaħ jinsabu f'`docs/20260511/`.

### Liċenzja / konfini

- Dokumenti tas-sors: xogħlijiet tal-gvern federali tal-Istati Uniti, fid-dominju pubbliku fl-Istati Uniti.
- Il-kodiċi proprju ta' din il-pjattaforma: ara `LICENSE`.
- Is-sit jibgħat `Tdm-Reservation: 1` u `X-Robots-Tag: noai, noimageai` — indiċjabbli minn magni tat-tiftix, magħżul barra mit-taħriġ/scraping tal-AI.
- Il-filmati tal-vidjow huma attribwiti lil DVIDS / AARO u mhumiex mitluba minn dan il-proġett.

Kwistjonijiet u PRs huma milqugħa. Jekk jogħġbok aqra `CLAUDE.md` u `docs/20260511/00-*` qabel tiftaħ bidliet strutturali.

