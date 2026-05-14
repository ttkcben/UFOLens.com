# GitHub – Innlegg 1 av 3 · Lanserings- / README-kunngjeringsblokk

**Bruk som:** ein GitHub Release-brødtekst, ein festa diskusjon, eller toppen av repo-README.
**Nøkkelord:** UAP, UFO, PURSUE-arkiv, avklassifiserte dokument, opne data, fulltekstsøk, OCR, maskinomsetjing, lokal LLM, Ollama, edge computing, offentleg API, Hono, TypeScript, Python
**Hyperlenker:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com – ei fleirspråkleg, søkbar plattform for PURSUE UAP-arkivet

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Kjeldefil:** https://www.war.gov/ufo

`ufolens.com` republiserer det amerikanske krigsdepartementets **PURSUE**-arkiv med avklassifiserte UAP / UFO-register som ein kunnskapsplattform: fulltekstsøk, maskinomsetjing på tvers av korpuset, kart- + tidslinjeutforsking, og ein offentleg JSON-API. Kjeldematerialet er arbeid utført av den amerikanske føderale regjeringa og er i USA i offentleg eige ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Dette prosjektet er **ikkje tilknytt den amerikanske regjeringa**, brukar ingen offisielle emblem, og reverserer aldri sladdingar.

### Arkitektur

```
Lokal maskin (Apple Silicon, privat IP)              Edge-nettverk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only kjerne)         worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (berre framover)  /{lang}/...   sider
  OCR: open-source motor (Tesseract CLI fallback)      /api/v1/...   offentleg API
  translate / NER: lokal LLM (Gemma via Ollama)        /admin        operatørkonsoll
  state: SQLite manifest                              støtta av: edge SQL DB, object
        │                                              storage (kjelde-PDF-ar), KV cache
        └── publiserer ein pakke: SQL + ressursmanifest + cache-tømmeliste ──┘
```

- **Null per-dokument sky-AI-kostnad.** OCR og omsetjing køyrer lokalt; den berre-framover-tilstandsmaskina (`oppdaga → lasta ned → ocr_ferdig → omsett → publisert`) garanterer at ingen dokument blir behandla på nytt med mindre det har endra seg.
- **Røyrleidningskjernen har ingen tredjepartsavhengnader** – parsing / manifest / deltamodular køyrer og testar på ein rein Python utan noko pip-installert; OCR/omsetjingsstega degraderer grasiøst når valfrie pakkar manglar.
- **Edge-sida** nyttar strenge tryggleikshovud + CSP (ingen `unsafe-inline`; inline JSON-LD sha256-festa), språkforhandling via `Accept-Language` + landtilknyting, ein 30-dagars KV-sidecache, og ein dagleg vedlikehalds-cron.
- **Inkrementelle oppdateringar:** ein deltadetektor samanliknar kjeldeindeksen og matar berre endringar tilbake i røyrleidninga.

### For utviklarar

Den offentlege API-en på https://www.ufolens.com/api/v1 returnerer dokument og metadata som JSON. Anonym tilgang er rate-avgrensa; be om ein nøkkel for forskar/utviklar-nivå. Sjå API-seksjonen på nettstaden for endepunkt og grenser.

### Status

Kode komplett; sida er deployert på https://www.ufolens.com. Produksjonsdatabasen blir fylt ved å køyre den offline røyrleidninga og publisere pakken framover (`cli_publish run --remote`). Fullstendige designdokument finst i `docs/20260511/`.

### Lisens / grenser

- Kjeldemateriale: arbeid av den amerikanske føderale regjeringa, offentleg eige i USA.
- Denne plattformas eigen kode: sjå `LICENSE`.
- Sida sender `Tdm-Reservation: 1` og `X-Robots-Tag: noai, noimageai` – indekserbar av søkemotorar, reservert mot AI-trening/skraping.
- Videoopptak er tilskrive DVIDS / AARO og blir ikkje gjort krav på av dette prosjektet.

Issues og PR-ar er velkomne. Ver vennleg og les `CLAUDE.md` og `docs/20260511/00-*` før du opnar strukturelle endringar.

