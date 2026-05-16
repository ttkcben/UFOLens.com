# GitHub — Indlæg 1 af 3 · Meddelelse om udgivelse / README

**Anvendelse:** Tekst til en GitHub Release, en fastgjort diskussion eller øverst i repoets README.
**Nøgleord:** UAP, UFO, PURSUE-arkiv, afklassificerede dokumenter, åbne data, fuldtekstsøgning, OCR, maskinoversættelse, lokal LLM, Ollama, edge computing, offentlig API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — en flersproget, søgbar platform for PURSUE UAP-arkivet

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Kildearkiv:** https://www.war.gov/ufo

`ufolens.com` genudgiver det amerikanske krigsministeriums **PURSUE**-arkiv med afklassificerede UAP / UFO-optegnelser som en vidensplatform: fuldtekstsøgning, maskinoversættelse på tværs af korpusset, kort- + tidslinjeudforskning og en offentlig JSON API. Kildedokumenterne er værker af den amerikanske føderale regering og er i USA offentlig ejendom ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dette projekt er **ikke tilknyttet den amerikanske regering**, bruger ingen officielle emblemer og fjerner aldrig redaktioner.

### Arkitektur

```
Lokal maskine (Apple Silicon, privat IP)            Edge-netværk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, kerne kun med stdlib)       worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (kun-fremadrettet) /{lang}/...   sider
  OCR: open-source-motor (Tesseract CLI fallback)      /api/v1/...   offentlig API
  translate / NER: lokal LLM (Gemma via Ollama)        /admin        operatørkonsol
  state: SQLite-manifest                             understøttet af: edge SQL DB, object
        │                                              storage (kilde-PDF'er), KV-cache
        └── udgiver et bundt: SQL + aktiv-manifest + cache-purge-liste ──┘
```

- **Nul cloud-AI-omkostninger pr. dokument.** OCR og oversættelse kører lokalt; den kun-fremadrettede tilstandsmaskine (`discovered → downloaded → ocr_done → translated → published`) garanterer, at intet dokument genbehandles, medmindre det har ændret sig.
- **Pipeline-kernen har ingen tredjepartsafhængigheder** — parsing- / manifest- / delta-moduler kører og testes på en ren Python uden noget pip-installeret; OCR/oversættelses-stadier nedbrydes elegant, når valgfrie pakker mangler.
- **Edge-sitet** anvender strenge sikkerhedsheadere + CSP (ingen `unsafe-inline`; inline JSON-LD er sha256-fastgjort), sprogforhandling via `Accept-Language` + landekortlægning, en 30-dages KV-sidecache og et dagligt vedligeholdelses-cron-job.
- **Inkrementelle opdateringer:** en delta-detektor differentierer kildeindekset og sender kun ændringer tilbage i pipelinen.

### For udviklere

Den offentlige API på https://www.ufolens.com/api/v1 returnerer dokumenter og metadata som JSON. Anonym adgang er rate-begrænset; anmod om en nøgle for forsker/udvikler-niveauer. Se API-sektionen på sitet for endepunkter og begrænsninger.

### Status

Koden er færdig; sitet er implementeret på https://www.ufolens.com. Produktionsdatabasen befolkes ved at køre den offline pipeline og udgive bundtet (`cli_publish run --remote`). Fuld design-dokumentation findes i `docs/20260511/`.

### Licens / afgrænsninger

- Kildedokumenter: Værker af den amerikanske føderale regering, offentlig ejendom i USA.
- Denne platforms egen kode: se `LICENSE`.
- Sitet sender `Tdm-Reservation: 1` og `X-Robots-Tag: noai, noimageai` — indekserbar af søgemaskiner, frameldt AI-træning/scraping.
- Videooptagelser tilskrives DVIDS / AARO og gøres der ikke krav på af dette projekt.

Issues og PRs er velkomne. Læs venligst `CLAUDE.md` og `docs/20260511/00-*`, før du åbner forslag til strukturelle ændringer.

