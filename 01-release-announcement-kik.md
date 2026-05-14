# GitHub — Kibandi 1 harĩ 3 · Kũmenyithania kwa Release / README

**Hũthĩra ta:** Mũbiri wa GitHub Release, Discussion ĩgwetetwo, kana igũrũ rĩa README ya repo.
**Ciugo cia bata:** UAP, UFO, mũthithũ wa PURSUE, ndumenti itarĩ na hitho, data yaherie, gũcaria kwa maandĩko mothe, OCR, ũtafsiri wa macini, LLM ya kũu, Ollama, edge computing, API yaherie, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — kĩuga gĩa ndimi nyingĩ, kĩa gũcaria maũndũ kĩa mũthithũ wa PURSUE UAP

**Rĩu rĩrĩ hewa-inĩ:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Mũthithũ wa kĩambĩrĩria:** https://www.war.gov/ufo

`ufolens.com` nĩ ĩcokerithagia kũmathiriria mũthithũ wa **PURSUE** wa Departmenti ya Mbaara ya U.S. wa marĩkodi ma UAP / UFO marĩa mataruĩrĩire ta kĩuga gĩa ũmenyo: gũcaria kwa maandĩko mothe, ũtafsiri wa macini thĩinĩ wa mũthithũ wothe, gũthuthuria na ramani + mũtaratara wa ihinda, na API ya JSON yaherie. Ndumenti cia kĩambĩrĩria nĩ mawĩra ma thirikari ya U.S. na thĩinĩ wa U.S. nĩ cia mũingĩ ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Mũradi ũyũ **ndũna uhusiano na thirikari ya U.S.**, ndũhũthagĩrĩ rũũri rwa kĩthirikari, na ndũrĩ hĩndĩ wagarũraga macacĩ.

### Mũhianĩre

```
Kombiyuta ya mũndũ (Apple Silicon, IP ya mũciĩ)      Network ya Edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Gũtirĩ garama ya cloud-AI kwa kila ndumenti.** OCR na ũtafsiri irutagĩrwo wĩra kũu; mũtaratara wa state wa forward-only (`discovered → downloaded → ocr_done → translated → published`) ũkinyithanagia atĩ gũtirĩ ndumenti ĩcokerithagio wĩra tiga o korwo nĩ ĩgarũrũkĩte.
- **Pipeline ya mũthingi ndĩna dependency ya gatatũ** — module cia gũthathaũra / manifest / delta irutaga wĩra na ikagerio na Python theru ĩtarĩ na kĩndũ kĩa pip-installed; tũtara twa OCR/ũtafsiri twĩhoyagirio wega rĩrĩa pakeji cia kwĩyendera itirĩ kuo.
- **Saitĩ ya Edge** ĩhũthagĩra hetha cia ũgitĩri wa hali ya igũrũ + CSP (gũtirĩ `unsafe-inline`; inline JSON-LD nĩ sha256-pinned), kwarĩrĩrio kwa rũrĩmĩ kũgerera `Accept-Language` + gũthima kwa bũrũri, cache ya peji ya KV ya thikũ 30, na cron ya wĩra wa kĩndũ o mũthenya.
- **Mogarũrũku ma kahora:** kĩonjoria kĩa delta kĩonaga ngurani harĩa kĩambĩrĩria kĩrĩ na gĩgacokeria o mogarũrũku tu thĩinĩ wa pipeline.

### Kũrĩ etia a sofutiwea

API ya mũingĩ thĩinĩ wa https://www.ufolens.com/api/v1 ĩcokeragia ndumenti na metadata ta JSON. Kũingĩra kũtarĩ na ũndeto nĩ kũigĩrĩirwo mĩhaka; hoya kĩhembe kĩa gĩkĩro kĩa athuthuria/etia. Rora gĩcunjĩ kĩa API thĩinĩ wa saitĩ nĩguo wone ciambĩki na mĩhaka.

### Ũrĩa kũhagaze

Wĩra wa gũthondeka kodi nĩ mũrĩku; saitĩ ĩrĩ hewa-inĩ thĩinĩ wa https://www.ufolens.com. Database ya production ĩiyũragio na kũruta wĩra pipeline ya offline na kũmathiriria bundle ĩrĩa ĩrũmĩrĩire (`cli_publish run --remote`). Ndumenti ciothe cia mũhianĩre irĩ thĩinĩ wa `docs/20260511/`.

### Ruhusa / mĩhaka

- Ndumenti cia kĩambĩrĩria: Mawĩra ma thirikari ya U.S., nĩ ya mũingĩ thĩinĩ wa U.S.
- Kodi ya kĩuga gĩkĩ: rora `LICENSE`.
- Saitĩ ĩtũmaga `Tdm-Reservation: 1` na `X-Robots-Tag: noai, noimageai` — ĩhotithagio gũciona nĩ injini cia gũcaria, no nĩ yetangĩte kuuma kũmenyererwo/kũguĩo nĩ AI.
- Video nĩ cia DVIDS / AARO na ti cia mũradi ũyũ.

Matatwa na PRs nĩ kwagĩrĩrwo. Tafadhali thoma `CLAUDE.md` na `docs/20260511/00-*` mbere ya kũhingũra mogarũrũku ma mũhianĩre.
