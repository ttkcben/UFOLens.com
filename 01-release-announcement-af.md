# GitHub — Pos 1 van 3 · Vrystelling / README-aankondigingsblok

**Gebruik as:** 'n GitHub Vrystelling-liggaam, 'n vasgespelde Bespreking, of die bokant van die repo se README.
**Sleutelwoorde:** UAP, UFO, PURSUE-argief, gedeklassifiseerde dokumente, oop data, voltekssoektog, OCR, masjienvertaling, plaaslike LLM, Ollama, randnetwerk-rekenaarverwerking, openbare API, Hono, TypeScript, Python
**Hipskakels:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — 'n veeltalige, soekbare platform vir die PURSUE UAP-argief

**Regstreeks:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Bronargief:** https://www.war.gov/ufo

`ufolens.com` herpubliseer die V.S. Departement van Oorlog se **PURSUE**-argief van gedeklassifiseerde UAP / UFO-rekords as 'n kennisplatform: voltekssoektog, masjienvertaling oor die hele korpus, kaart- en tydlynverkenning, en 'n openbare JSON API. Brondokumente is werke van die Amerikaanse federale regering en is binne die VSA in die openbare domein ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Hierdie projek is **nie geaffilieer met die V.S. regering nie**, gebruik geen amptelike kentekens nie, en maak nooit redaksies ongedaan nie.

### Argitektuur

```
Plaaslike masjien (Apple Silicon, residensiële IP)   Randnetwerk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-enigste kern)        worker/  (TypeScript, Hono.js)
  haal → OCR → vertaal → publiseer (slegs-vorentoe)     /{lang}/...   bladsye
  OCR: oopbron-enjin (Tesseract CLI terugval)          /api/v1/...   openbare API
  vertaal / NER: plaaslike LLM (Gemma via Ollama)      /admin        operateur-konsole
  toestand: SQLite manifes                           gerugsteun deur: rand-SQL DB, objek-
        │                                              berging (bron-PDF's), KV-kas
        └── publiseer 'n bundel: SQL + bate-manifes + kas-skoonmaaklys ──┘
```

- **Nul wolk-KI-koste per dokument.** OCR en vertaling loop plaaslik; die slegs-vorentoe toestandsmasjien (`ontdek → afgelaai → ocr_gedoen → vertaal → gepubliseer`) waarborg dat geen dokument herverwerk word nie, tensy dit verander het.
- **Pyplynkern het geen derdeparty-afhanklikhede nie** — ontledings- / manifes- / delta-modules loop en toets op 'n skoon Python met niks wat deur pip geïnstalleer is nie; OCR/vertaling-stadia gradeer grasieus af wanneer opsionele pakkette afwesig is.
- **Randwerf** pas streng sekuriteitsopskrifte + CSP toe (geen `unsafe-inline`; inlyn JSON-LD is sha256-vasgespeld), taalonderhandeling via `Accept-Language` + landkartering, 'n 30-dae KV-bladsykas, en 'n daaglikse instandhoudings-cron.
- **Inkrementele opdaterings:** 'n delta-verklikker vergelyk die bronindeks en voer slegs veranderinge terug in die pyplyn.

### Vir ontwikkelaars

Die openbare API by https://www.ufolens.com/api/v1 lewer dokumente en metadata as JSON. Anonieme toegang is tempo-beperk; versoek 'n sleutel vir navorser/ontwikkelaar-vlakke. Sien die API-afdeling op die werf vir eindpunte en limiete.

### Status

Kode voltooi; werf ontplooi by https://www.ufolens.com. Die produksiedatabasis word bevolk deur die vanlyn pyplyn te loop en die bundel vorentoe te publiseer (`cli_publish run --remote`). Volledige ontwerpdokumente is in `docs/20260511/`.

### Lisensie / grense

- Brondokumente: Werke van die Amerikaanse federale regering, openbare domein binne die VSA.
- Hierdie platform se eie kode: sien `LICENSE`.
- Die werf stuur `Tdm-Reservation: 1` en `X-Robots-Tag: noai, noimageai` — indekseerbaar deur soekenjins, onttrek aan KI-opleiding/skraping.
- Videomateriaal word toegeskryf aan DVIDS / AARO en word nie deur hierdie projek geëis nie.

Kwessies en PRs is welkom. Lees asseblief `CLAUDE.md` en `docs/20260511/00-*` voordat u strukturele veranderinge open.

