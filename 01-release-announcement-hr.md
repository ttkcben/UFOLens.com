# GitHub — Objava 1 od 3 · Izdanje / najavni blok za README

**Koristiti kao:** tijelo GitHub izdanja, prikvačenu raspravu ili vrh repozitorijskog README-a.
**Ključne riječi:** UAP, NLO, arhiva PURSUE, deklasificirani dokumenti, otvoreni podaci, pretraživanje cijelog teksta, OCR, strojno prevođenje, lokalni LLM, Ollama, rubno računalstvo, javni API, Hono, TypeScript, Python
**Poveznice:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — višejezična, pretraživa platforma za arhivu PURSUE UAP

**Uživo:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Izvorna arhiva:** https://www.war.gov/ufo

`ufolens.com` ponovno objavljuje arhivu deklasificiranih UAP / NLO zapisa **PURSUE** američkog Ministarstva rata kao platformu znanja: pretraživanje cijelog teksta, strojno prevođenje cijelog korpusa, istraživanje karte i vremenske crte te javni JSON API. Izvorni dokumenti su djela savezne vlade SAD-a i unutar SAD-a su u javnoj domeni ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ovaj projekt **nije povezan s vladom SAD-a**, ne koristi službene oznake i nikada ne poništava redakcije.

### Arhitektura

```
Lokalno računalo (Apple Silicon, kućni IP)     Rubna mreža
─────────────────────────────────────────      ─────────────────────────
pipeline/  (Python 3.10, jezgra samo sa stdlib) worker/  (TypeScript, Hono.js)
  dohvati → OCR → prevedi → objavi (samo naprijed) /{lang}/...   stranice
  OCR: open-source mehanizam (Tesseract CLI kao rezerva) /api/v1/...   javni API
  prevođenje / NER: lokalni LLM (Gemma putem Ollama)    /admin        konzola operatera
  stanje: SQLite manifest                          podržano s: rubna SQL baza podataka, pohrana
        │                                          objekata (izvorni PDF-ovi), KV predmemorija
        └── objavljuje paket: SQL + manifest resursa + popis za čišćenje predmemorije ──┘
```

- **Nulti trošak po dokumentu za AI u oblaku.** OCR i prevođenje se izvršavaju lokalno; stroj stanja koji ide samo naprijed (`otkriven → preuzet → ocr_gotov → preveden → objavljen`) jamči da se nijedan dokument ne obrađuje ponovno osim ako se nije promijenio.
- **Jezgra cjevovoda nema ovisnosti o trećim stranama** — moduli za parsiranje / manifest / deltu rade i testiraju se na čistom Pythonu bez ičega instaliranog putem pipa; faze OCR-a/prevođenja graciozno se degradiraju kada su opcionalni paketi odsutni.
- **Rubna stranica** primjenjuje stroga sigurnosna zaglavlja + CSP (bez `unsafe-inline`; inline JSON-LD sha256-prikvačen), jezično pregovaranje putem `Accept-Language` + mapiranje zemalja, 30-dnevnu KV predmemoriju stranica i dnevni cron za održavanje.
- **Inkrementalna ažuriranja:** detektor delte uspoređuje izvorni indeks i unosi samo promjene natrag u cjevovod.

### Za programere

Javni API na https://www.ufolens.com/api/v1 vraća dokumente i metapodatke kao JSON. Anonimni pristup je ograničen po broju zahtjeva; zatražite ključ za istraživačke/razvojne razine. Pogledajte odjeljak API na stranici za krajnje točke i ograničenja.

### Status

Kod je dovršen; stranica je postavljena na https://www.ufolens.com. Produkcijska baza podataka popunjava se pokretanjem izvanmrežnog cjevovoda i objavljivanjem paketa prema naprijed (`cli_publish run --remote`). Potpuna projektna dokumentacija nalazi se u `docs/20260511/`.

### Licenca / granice

- Izvorni dokumenti: djela savezne vlade SAD-a, u javnoj domeni unutar SAD-a.
- Vlastiti kod ove platforme: pogledajte `LICENSE`.
- Stranica šalje `Tdm-Reservation: 1` i `X-Robots-Tag: noai, noimageai` — indeksira se za pretraživače, isključen iz AI obuke/struganja.
- Video materijali pripisuju se DVIDS / AARO i ovaj projekt ne polaže pravo na njih.

Problemi i PR-ovi su dobrodošli. Molimo pročitajte `CLAUDE.md` i `docs/20260511/00-*` prije otvaranja strukturnih promjena.

