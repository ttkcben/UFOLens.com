# GitHub — Post 1 de 3 · Anònçio de Release / Blòcco pe-o README

**Dêuvo:** cómme còrpo de 'na Release de GitHub, 'na Discusción fisâ, ò in çìmma a-o README do repo.
**Paròlle ciave:** UAP, UFO, archivio PURSUE, documenti declassificæ, dæti averto, riçerca full-text, OCR, traduçión aotomàtica, LLM locâle, Ollama, edge computing, API pùblica, Hono, TypeScript, Python
**Colegamenti ipertestoâli:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — 'na piattaforma multilingua e riçercabile pe l'archivio PURSUE UAP

**In diretta:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Archivio da-a vivàgna:** https://www.war.gov/ufo

`ufolens.com` o tórna a publicâ l'archivio **PURSUE** do Dipartimento da Goæra di Stati Unîi de documenti UAP / UFO declassificæ cómme 'na piattaforma de conoscénsa: riçerca full-text, traduçión aotomàtica in sce tutto o corpus, esploraçión de màppe e cronologîe, e 'n'API pùblica JSON. I documenti da-a vivàgna són travàggi do govèrno federâle di Stati Unîi e, into teritöio di Stati Unîi, són de domìnio pùblico ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Sto progètto o **no l'é afiliòu a-o govèrno di Stati Unîi**, o no dêuvia nisciùn insìgna ofiçiâ, e o no revèrse mâi e redaçioìn.

### Architetûa

```
Màchina locâle (Apple Silicon, IP rescidençiâle)        Ræ a-o bòrdo (edge network)
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, core solo-stdlib)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (solo in avanti)    /{lang}/...   pàgine
  OCR: motô open-source (fallback Tesseract CLI)     /api/v1/...   API pùblica
  translate / NER: LLM locâle (Gemma via Ollama)        /admin        console de l'operatô
  stâto: manifesto SQLite                             suportòu da: DB SQL a-o bòrdo,
        │                                              archiviaçión de ògètti (PDF da-a vivàgna),
        └── pùblica 'n pachetto: SQL + manifesto asset + lista de purgaçión cache ──┘                                             cache KV
```

- **Còsto zero de AI in cloud pe ciaschedùn documento.** L'OCR e a traduçión gîan in locâle; a màchina a stâti solo in avanti (`descoerto → descaregòu → ocr_fæto → tradûto → publicòu`) a garantìsce che nisciùn documento o segge tórna elaboròu a mêno ch'o no segge cangiòu.
- **O còr do pipeline o no l'à dipendénse de tèrse pàrte** — i mòdoli de anàlixi / manifesto / delta gîan e són testæ in sce 'n Python néto sénsa nìnte de instalòu con pip; e fâze de OCR/traduçión degràdan con gràçia quànde i pachetti òpçionâli són asénti.
- **O scîto a-o bòrdo** o l'àplica rigorozi header de seguéssa + CSP (nisciùn `unsafe-inline`; i JSON-LD inlìnia són fisæ con sha256), negociaçión da léngoa co-o `Accept-Language` + mapatûa do pàize, 'na cache de pàgina KV de 30 giórni, e 'n cron giornaliêro de manutençión.
- **Agiórnamenti incrementâli:** 'n rilevatô de delta o contròlla e diferénse de l'ìndice da vivàgna e o fornìsce a-o pipeline sôlo i cangiaménti.

### Pe-i svilupatoî

L'API pùblica a https://www.ufolens.com/api/v1 a dà inderrê i documenti e i metadæti cómme JSON. L'acèsso anònimo o l'é limitòu pe-o nùmero de domànde; domandæ 'na ciâve pe-i livèlli de riçercatô/svilupatô. Vedde a seçión API in sciô scîto pe-i endpoint e i lìmitei.

### Stâto

Còdice conpletòu; scîto distriboîo in sce https://www.ufolens.com. O database de produçión o l'é caregòu faxendo gjâ o pipeline offline e publicàndo o pachetto in avanti (`cli_publish run --remote`). A progetaçión conplêta a se trêuva in `docs/20260511/`.

### Licénsa / confìn

- Documenti da-a vivàgna: travàggi do govèrno federâle di Stati Unîi, de domìnio pùblico inti Stati Unîi.
- O còdice pròpio de sta piattaforma: vedde `LICENSE`.
- O scîto o manda `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — indicisàbile da-i motoî de riçerca, disattivòu da-a formaçión/scraping de AI.
- I filmâti són atriboîi a DVIDS / AARO e no són reclamæ da sto progètto.

Issues e PRs són benvegnûi. Pe piiaxéi, lezéi `CLAUDE.md` e `docs/20260511/00-*` prìmma de propón-e cangiaménti struturâli.

