# GitHub — 1/3 Argitalpena · Bertsioaren / README iragarki blokea

**Erabiltzeko modua:** GitHub Release baten gorputz gisa, ainguratutako Discussion gisa, edo repo-aren README fitxategiaren goialdean.
**Gako-hitzak:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hiperestekak:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP artxiborako plataforma eleaniztun eta bilagarria

**Zuzenean:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Iturburu artxiboa:** https://www.war.gov/ufo

`ufolens.com`-ek AEBetako Gerra Sailaren **PURSUE** artxiboa (UAP / UFO erregistro desklasifikatuak) berrargitaratzen du ezagutza plataforma gisa: testu osoko bilaketa, corpus osoan zeharreko itzulpen automatikoa, mapen + kronogramen esplorazioa, eta JSON API publiko bat. Iturburuko dokumentuak AEBetako gobernu federalaren lanak dira eta AEBen barruan jabari publikokoak dira ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Proiektu hau **ez dago AEBetako gobernuarekin afiliatuta**, ez du intsignia ofizialik erabiltzen, eta inoiz ez ditu erredakzioak lehengoratzen.

### Arkitektura

```
Makina lokala (Apple Silicon, IP egoiliarra)          Edge sarea
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (aurreranzkoa soilik)    /{lang}/...   orriak
  OCR: kode irekiko motorra (Tesseract CLI fallback)     /api/v1/...   API publikoa
  translate / NER: LLM lokala (Gemma Ollama bidez)        /admin        operadorearen kontsola
  egoera: SQLite manifestua                             honen babesarekin: edge SQL DB, objektu
        │                                              biltegia (iturburuko PDFak), KV cachea
        └── sorta bat argitaratzen du: SQL + baliabide-manifestua + cache-garbiketa zerrenda ──┘
```

- **Zero kostu dokumentu bakoitzeko hodeiko AI-an.** OCR eta itzulpena lokalean exekutatzen dira; aurreranzko soilik den egoera-makinak (`discovered → downloaded → ocr_done → translated → published`) bermatzen du dokumenturik ez dela berriro prozesatzen, aldatu ez bada.
- **Pipeline-aren muinak ez du hirugarrenen menpekotasunik** — parsing / manifest / delta moduluak Python garbi batean exekutatzen eta probatzen dira ezer pip-ekin instalatu gabe; OCR/itzulpen faseek ondo degradatzen dute aukerako paketeak falta direnean.
- **Edge guneak** segurtasun goiburu zorrotzak + CSP aplikatzen ditu (`unsafe-inline` gabe; lerroko JSON-LD sha256-rekin finkatuta dago), hizkuntza-negoziazioa `Accept-Language` bidez + herrialde-mapaketa, 30 eguneko KV orri-cachea, eta eguneroko mantentze-cron bat.
- **Eguneratze gehikorrak:** delta detektagailu batek iturburu-indizearen diff-ak egiten ditu eta aldaketak soilik sartzen ditu berriro pipeline-an.

### Garatzaileentzat

https://www.ufolens.com/api/v1 helbideko API publikoak dokumentuak eta metadatuak JSON formatuan itzultzen ditu. Sarbide anonimoa tasa-mugatua da; eskatu gako bat ikertzaile/garatzaile mailetarako. Ikusi guneko API atala endpoint-ak eta mugak ezagutzeko.

### Egoera

Kodea osatuta; gunea https://www.ufolens.com helbidean hedatuta. Ekoizpeneko datu-basea lineaz kanpoko pipeline-a exekutatuz eta sorta aurrera argitaratuz betetzen da (`cli_publish run --remote`). Diseinu-dokumentu osoak `docs/20260511/` karpetan daude.

### Lizentzia / mugak

- Iturburuko dokumentuak: AEBetako gobernu federalaren lanak, jabari publikokoak AEBen barruan.
- Plataforma honen kode propioa: ikus `LICENSE`.
- Guneak `Tdm-Reservation: 1` eta `X-Robots-Tag: noai, noimageai` bidaltzen ditu — bilaketa-motorrek indexatu dezakete, AI trebakuntzatik/scraping-etik kanpo utzita.
- Bideo-materiala DVIDS / AARO-ri egozten zaio eta proiektu honek ez du bere gain hartzen.

Issue-ak eta PR-ak ongi etorriak dira. Mesedez, irakurri `CLAUDE.md` eta `docs/20260511/00-*` egiturazko aldaketak ireki aurretik.
