# GitHub — Post 1 a 3 · Kemeneth a-dro dhe Dhyllans / Block README

**Devnydh avel:** korf GitHub Release, Keskows pynnys, po penn an repo README.
**Geryow alhwedh:** UAP, UFO, arhive PURSUE, dokumentow digelennys, data yger, hwithrans tekst leun, OCR, treylyans jynn, LLM leel, Ollama, amontya an amal, API poblek, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — platform lies-yethek, hwithradow rag arhive PURSUE UAP

**Yn fyw:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Arhive an vammen:** https://www.war.gov/ufo

`ufolens.com` a-dasdhyll arhive **PURSUE** a govadhow UAP / UFO digelennys gans Ranngylgh Vresel an S.U. avel platform godhvos: hwithrans tekst leun, treylyans jynn dres an korpus, hwithra map + amserlinenn, hag API JSON poblek. Dokumentow an mammen yw oberow a wovernans kres an S.U. hag a-ji dhe'n S.U. ymons y'n parth poblek ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). An ragdres ma **nyns yw kevrynnys gans governans an S.U.**, ny wra devnydh a arwodhow sodhogel, ha bythkweth ny wra treylya digelennow.

### Pennserneth

```
Jynn leel (Apple Silicon, IP trigys)                Rosweyth amal
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, kres hepken-stdlib)         worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (rag-onlys)       /{lang}/...   folennow
  OCR: jynn-yger mammen (Tesseract CLI a-dhelergh)      /api/v1/...   API poblek
  translate / NER: LLM leel (Gemma dre Ollama)          /admin        konsol an oberor
  studh: manifest SQLite                              skoodhys gans: DB SQL amal,
        │                                              stokkas obyek (PDFow mammen), cache KV
        └── a dhyllo bondel: SQL + manifest asedhow + rol-purha cache ──┘
```

- **Kost mann dre dhokument rag AI-kommol.** OCR ha treylyans a rol war an le; an jynn studh rag-onlys (`diskudhys → iskargys → ocr_gwres → treylys → dyllys`) a warant na vo dokument dasoberys marnas ev dhe janjya.
- **Nyns eus kreghenson tryja-parti dhe gres an biblinell** — an modylow diwosa / manifest / delta a rol hag a brof war Python glan heb travyth `pip`-installlys; an gradhow OCR/treylyans a dhasgrad yn jentyl pan vo pakkys dewisadow anes.
- **An savle amal** a skrif pennskrifow diogeledh + CSP strick (nyns eus `unsafe-inline`; JSON-LD a-linen yw sha256-pynnys), kesvreusyans yeth dre `Accept-Language` + mappa bro, cache folennow KV 30-dydh, ha cron gorweyth-chi pempenydhyek.
- **Nowedhansow ynkressel:** diskudhher delta a wra diberth endeks an vammen ha ny wra fydhya marnas an chanjyow yn-dhelergh y'n biblinell.

### Rag displegyoryon

An API poblek war https://www.ufolens.com/api/v1 a-dhegresso dokumentow ha metadata avel JSON. Hedh heb-hanow yw kevys-tremenys; govynn rag alhwedh rag nivelyow hwithrer/displegyer. Gwel an rann API war an savle rag poyntow-gorfen ha finyow.

### Studh

Kod leun; savle delivrys war https://www.ufolens.com. An database askorrans yw poblennys dre rollya an biblinell a-allenn ha dyllo an bondel yn-rag (`cli_publish run --remote`). Dokumentow desin leun a drig yn `docs/20260511/`.

### Kummyas / oryon

- Dokumentow mammen: Oberow governans kres an S.U., parth poblek a-ji dhe'n S.U.
- Kod an platform ma y honan: gwel `LICENSE`.
- An savle a dhannvon `Tdm-Reservation: 1` ha `X-Robots-Tag: noai, noimageai` — endeksadow gans jynnow hwilas, dewisys-mes a dhyskians/kravans AI.
- An fylmow video yw kevys dhe DVIDS / AARO ha nyns yns i arghys gans an ragdres ma.

Kudynnow ha PRow yw wolkom. Gwra lenn `CLAUDE.md` ha `docs/20260511/00-*` kyns ygeri chanjyow framweythek.

