# GitHub — Objava 1 od 3 · Izdanje / blok za najavu u README

**Koristiti kao:** tijelo GitHub izdanja, zakačenu diskusiju ili na vrhu README repozitorija.
**Ključne riječi:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hiperveze:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — višejezična, pretraživa platforma za PURSUE UAP arhiv

**Uživo:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Izvorni arhiv:** https://www.war.gov/ufo

`ufolens.com` ponovno objavljuje **PURSUE** arhiv deklasificiranih UAP / UFO zapisa Ministarstva rata SAD-a kao platformu znanja: pretraživanje cijelog teksta, mašinsko prevođenje cijelog korpusa, istraživanje putem karte i vremenske linije, te javni JSON API. Izvorni dokumenti su djela federalne vlade SAD-a i unutar SAD-a su u javnoj domeni ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ovaj projekt **nije povezan s vladom SAD-a**, ne koristi nikakve službene oznake i nikada ne poništava redakcije.

### Arhitektura

```
Lokalna mašina (Apple Silicon, rezidencijalni IP)     Edge mreža
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, jezgra samo sa stdlib)      worker/  (TypeScript, Hono.js)
  dohvati → OCR → prevedi → objavi (samo naprijed)     /{lang}/...   stranice
  OCR: open-source engine (Tesseract CLI rezervni)    /api/v1/...   javni API
  prevođenje / NER: lokalni LLM (Gemma preko Ollama)   /admin        operatorska konzola
  stanje: SQLite manifest                            podržano sa: edge SQL DB, objektno
        │                                              skladište (izvorni PDF-ovi), KV keš
        └── objavljuje paket: SQL + manifest asseta + lista za čišćenje keša ──┘
```

- **Nula troškova po dokumentu za AI u oblaku.** OCR i prevođenje se izvršavaju lokalno; stanje mašine "samo naprijed" (`discovered → downloaded → ocr_done → translated → published`) garantuje da se nijedan dokument neće ponovo obrađivati osim ako se nije promijenio.
- **Jezgra pipelinea nema ovisnosti o trećim stranama** — moduli za parsiranje / manifest / deltu se izvršavaju i testiraju na čistom Pythonu bez ičega instaliranog putem pip-a; faze OCR/prevođenja se graciozno degradiraju kada opcionalni paketi nisu prisutni.
- **Edge stranica** primjenjuje stroge sigurnosne headere + CSP (bez `unsafe-inline`; inline JSON-LD sha256-prikačen), pregovaranje jezika putem `Accept-Language` + mapiranje zemalja, 30-dnevni KV keš stranica i dnevni cron za održavanje.
- **Inkrementalna ažuriranja:** detektor delte upoređuje izvorni indeks i šalje samo promjene natrag u pipeline.

### Za programere

Javni API na https://www.ufolens.com/api/v1 vraća dokumente i metapodatke kao JSON. Anonimni pristup je ograničen po broju zahtjeva; zatražite ključ za istraživačke/razvojne nivoe. Pogledajte odjeljak API na stranici za endpointe i ograničenja.

### Status

Kod je završen; stranica je postavljena na https://www.ufolens.com. Produkcijska baza podataka se popunjava pokretanjem offline pipelinea i objavljivanjem paketa (`cli_publish run --remote`). Kompletna projektna dokumentacija se nalazi u `docs/20260511/`.

### Licenca / granice

- Izvorni dokumenti: djela federalne vlade SAD-a, u javnoj domeni unutar SAD-a.
- Kod ove platforme: pogledajte `LICENSE`.
- Stranica šalje `Tdm-Reservation: 1` i `X-Robots-Tag: noai, noimageai` — indeksabilno za pretraživače, isključeno iz AI treninga/struganja.
- Video snimci su pripisani DVIDS / AARO i ovaj projekt ne polaže pravo na njih.

Prijave grešaka i PR-ovi su dobrodošli. Molimo pročitajte `CLAUDE.md` i `docs/20260511/00-*` prije otvaranja strukturalnih promjena.
