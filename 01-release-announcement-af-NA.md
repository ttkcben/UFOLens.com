# GitHub — Pos 1 van 3 · Vrystelling / README aankondigingsblok

**Gebruik as:** 'n GitHub Vrystelling-liggaam, 'n vasgepende Bespreking, of bo-aan die repo se README.
**Sleutelwoorde:** UAP, UFO, PURSUE-argief, gedeklassifiseerde dokumente, oop data, voltekssoektog, OCR, masjienvertaling, plaaslike LLM, Ollama, randnetwerkrekenaarkunde, openbare API, Hono, TypeScript, Python
**Hipskakels:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — 'n meertalige, soekbare platform vir die PURSUE UAP-argief

**Regstreeks:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Bronargief:** https://www.war.gov/ufo

`ufolens.com` herpubliseer die V.S. Oorlogsdepartement se **PURSUE**-argief van gedeklassifiseerde UAP / UFO-rekords as 'n kennisplatform: voltekssoektog, masjienvertaling oor die hele korpus, kaart- en tydlynverkenning, en 'n openbare JSON API. Brondokumente is werke van die V.S. federale regering en is binne die V.S. in die openbare domein ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Hierdie projek is **nie geaffilieer met die V.S. regering nie**, gebruik geen amptelike insignia nie, en keer nooit redaksies om nie.

### Argitektuur

```
Plaaslike masjien (Apple Silicon, residensiële IP)        Randnetwerk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, slegs-stdlib kern)            worker/  (TypeScript, Hono.js)
  haal → OCR → vertaal → publiseer (slegs-vorentoe)      /{lang}/...   bladsye
  OCR: oopbron-enjin (Tesseract CLI-terugval)            /api/v1/...   openbare API
  vertaal / NER: plaaslike LLM (Gemma via Ollama)        /admin        operateur-konsole
  toestand: SQLite-manifes                             ondersteun deur: rand-SQL-DB, objek-
        │                                              berging (bron-PDF's), KV-kas
        └── publiseer 'n bundel: SQL + bate-manifes + kas-skoonmaaklys ──┘
```

- **Nul koste per dokument vir wolk-KI.** OCR en vertaling loop plaaslik; die slegs-vorentoe-toestandmasjien (`ontdek → afgelaai → ocr_gedoen → vertaal → gepubliseer`) waarborg dat geen dokument herverwerk word tensy dit verander het nie.
- **Die pyplyn-kern het geen derdeparty-afhanklikhede nie** — ontleding / manifes / delta-modules loop en toets op 'n skoon Python met niks wat met pip geïnstalleer is nie; OCR/vertaal-stadia gradeer grasieus af wanneer opsionele pakkette afwesig is.
- **Randwebwerf** pas streng sekuriteitsheaders + CSP toe (geen `unsafe-inline`; inlyn JSON-LD is sha256-vasgepen), taalonderhandeling via `Accept-Language` + landkartering, 'n 30-dae KV-bladkas, en 'n daaglikse skoonmaak-cron.
- **Inkrementele opdaterings:** 'n delta-opspoorder vergelyk die bronindeks en voer slegs veranderinge terug in die pyplyn.

### Vir ontwikkelaars

Die openbare API by https://www.ufolens.com/api/v1 lewer dokumente en metadata as JSON. Anonieme toegang is tempo-beperk; versoek 'n sleutel vir navorser/ontwikkelaar-vlakke. Sien die API-afdeling op die webwerf vir eindpunte en limiete.

### Status

Kode voltooi; webwerf ontplooi by https://www.ufolens.com. Die produksiedatabasis word bevolk deur die vanlyn pyplyn te laat loop en die bundel vorentoe te publiseer (`cli_publish run --remote`). Volledige ontwerpdokumente is in `docs/20260511/`.

### Lisensie / grense

- Brondokumente: V.S. federale regeringswerke, openbare domein binne die V.S.
- Hierdie platform se eie kode: sien `LICENSE`.
- Die webwerf stuur `Tdm-Reservation: 1` en `X-Robots-Tag: noai, noimageai` — indekseerbaar deur soekenjins, onttrek van KI-opleiding/skraping.
- Videomateriaal word toegeskryf aan DVIDS / AARO en word nie deur hierdie projek geëis nie.

Kwessies en PRs is welkom. Lees asseblief `CLAUDE.md` en `docs/20260511/00-*` voordat u strukturele veranderinge oopmaak.

