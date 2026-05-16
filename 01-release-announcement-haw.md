# GitHub — Helu 1 o 3 · Hoʻolaha Hoʻokuʻu / README

**E hoʻohana ma ke ʻano he:** kino hoʻokuʻu GitHub, Kūkākūkā i hoʻopaʻa ʻia, a i ʻole ka piko o ka README repo.
**Nā huaʻōlelo koʻikoʻi:** UAP, UFO, PURSUE archive, nā palapala i hoʻokaʻawale ʻia, ka ʻikepili ākea, hulina kikokikona piha, OCR, unuhi mīkini, LLM kūloko, Ollama, edge computing, API ākea, Hono, TypeScript, Python
**Nā loulou pūnaewele:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — he paepae ʻōlelo nui, hiki ke hulina no ka waihona PURSUE UAP

**Ola:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Waihona kumu:** https://www.war.gov/ufo

Hoʻopuka hou ʻo `ufolens.com` i ka waihona **PURSUE** o ka U.S. War Department o nā moʻolelo UAP / UFO i hoʻokaʻawale ʻia ma ke ʻano he paepae ʻike: hulina kikokikona piha, unuhi mīkini ma luna o ka palapala, palapala ʻāina + nānā manawa, a me kahi JSON API ākea. He mau hana nā palapala kumu a ke aupuni federal o ʻAmelika Hui Pū ʻIa a ma loko o ʻAmelika Hui Pū ʻIa he waiwai lehulehu ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). ʻAʻole pili kēia papahana **me ke aupuni o ʻAmelika Hui Pū ʻIa**, ʻaʻole hoʻohana i nā hōʻailona kūhelu, a ʻaʻole hoʻi e hoʻohuli i nā hoʻoponopono.

### Kūkulu

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

- **ʻAʻohe kumu kūʻai cloud-AI no kēlā me kēia palapala.** Holo ka OCR a me ka unuhi ma ka wahi kūloko; hoʻopaʻa ka mīkini mokuʻāina i mua wale (`discovered → downloaded → ocr_done → translated → published`) ʻaʻole e hoʻoponopono hou ʻia kahi palapala ke ʻole ia i loli.
- **ʻAʻohe hilinaʻi ʻekolu o ke kumu pipeline** — holo a hoʻāʻo nā modules parsing / manifest / delta ma kahi Python maʻemaʻe me ka ʻole o ka pip-installed; hōʻemi maikaʻi nā pae OCR/translation ke nele nā pūʻolo koho.
- Hoʻopili ka **pūnaewele Edge** i nā poʻo palekana koʻikoʻi + CSP (ʻaʻohe `unsafe-inline`; hoʻopaʻa ʻia ka JSON-LD inline sha256), kūkākūkā ʻōlelo ma o `Accept-Language` + palapala ʻāina, kahi KV page cache 30 lā, a me kahi cron mālama hale i kēlā me kēia lā.
- **Nā mea hou hoʻonui:** hoʻokaʻawale ka delta detector i ka papa kuhikuhi kumu a hānai wale i nā loli i ka pipeline.

### No nā mea hoʻomohala

Hoʻihoʻi mai ka API ākea ma https://www.ufolens.com/api/v1 i nā palapala a me nā metadata ma ke ʻano he JSON. Hoʻopaʻa ʻia ke komo ʻana o ka mea ʻike ʻole i ka helu; e noi i kī no nā pae noiʻi/mea hoʻomohala. E ʻike i ka ʻāpana API ma ka pūnaewele no nā wahi hopena a me nā palena.

### Kūlana

Ua pau ke code; ua hoʻonoho ʻia ka pūnaewele ma https://www.ufolens.com. Hoʻopiha ʻia ka waihona ʻikepili hana ma o ka holo ʻana i ka pipeline offline a me ka hoʻopuka ʻana i ka pūʻulu i mua (`cli_publish run --remote`). Aia nā palapala hoʻolālā piha ma `docs/20260511/`.

### Laikini / palena

- Nā palapala kumu: nā hana a ke aupuni federal o ʻAmelika Hui Pū ʻIa, waiwai lehulehu ma loko o ʻAmelika Hui Pū ʻIa.
- Ke code ponoʻī o kēia paepae: e ʻike i `LICENSE`.
- Hoʻouna ka pūnaewele `Tdm-Reservation: 1` a me `X-Robots-Tag: noai, noimageai` — hiki ke hulina e nā ʻenekini hulina, ua koho ʻia mai ke aʻo ʻana AI / ʻohi ʻikepili.
- Hāʻawi ʻia nā kiʻi wikiō iā DVIDS / AARO a ʻaʻole i koi ʻia e kēia papahana.

Hoʻokipa ʻia nā pilikia a me nā PR. E ʻoluʻolu e heluhelu i `CLAUDE.md` a me `docs/20260511/00-*` ma mua o ka wehe ʻana i nā loli hoʻolālā.

