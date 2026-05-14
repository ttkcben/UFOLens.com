# GitHub — Póstrzédny pòst 1 z 3 · Ogłoszenié ò wëdanim / blok ogłoszeniowi README

**Ùżëj jakno:** tekst wëdaniô GitHub, przëpiktą diskùsëją abò górny dzél repòzytorium README.
**Klëczowé słowa:** UAP, UFO, archiwùm PURSUE, òdtajnioné dokùmentë, òtemkłé dané, szëkônié w całim tekscë, OCR, maszinowé dolmaczenié, môlowy LLM, Ollama, edge computing, publiczny API, Hono, TypeScript, Python
**Hiperłącza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — wielojãzycznô, przeszëkiwalnô platfòrma dlô archiwùm PURSUE UAP

**Na żëwò:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Zdrzódłowé archiwùm:** https://www.war.gov/ufo

`ufolens.com` na nowò pùblikùje archiwùm **PURSUE** Departamentu Wòjnë USA z òdtajnionyma rapòrtama ò UAP / UFO jakno platfòrmã wiédzë: szëkônié w całim tekscë, maszinowé dolmaczenié w całim kòrpusie, badérowanié na mapie + czasowim lënkù ë publiczny API JSON. Zdrzódłowé dokùmentë są robòtama federalnégò rządu USA ë w USA są w publiczny domenë ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Nen projekt **nie je pòłączony z rządem USA**, nie ùżiwô òficjalnëch insygniów ë nigdë nie òdwrôcô redakcëji.

### Architektura

```
Lokalnô maszina (Apple Silicon, domôcy IP)        Siec krawędziowô (Edge network)
──────────────────────────────────────────          ──────────────────────────
pipeline/  (Python 3.10, rdzeń blós ze stdlib)     worker/  (TypeScript, Hono.js)
  pòbiérz → OCR → dolmacz → pùblikùj (blós do przodku)   /{lang}/...   stronë
  OCR: òtemkłi nédżi (Tesseract CLI jakno alternatiwa) /api/v1/...   publiczny API
  dolmaczenié / NER: môlowy LLM (Gemma przez Ollama)  /admin        kònsola òperatora
  stan: manifezt SQLite                            zabezpieczony przez: krawędziową bazę SQL,
        │                                            magazyn òbiektów (zdrzódłowé PDFë), pòdrãczną pamiãc KV
        └── pùblikùje paczkã: SQL + manifezt aktywów + lësta do czëszczeniô pòdrãczny pamiãcë ──┘
```

- **Zerowi kòszt za dokùment w chmùrze AI.** OCR ë dolmaczenié dzejają môlowò; automat stanów blós do przodku (`òdkrëté → pòbróné → ocr_zrobioné → zrobioné_dolmaczenié → opùblikòwóné`) gwarentëje, że żaden dokùment nie je znowù przerôbiany, chëbà że sã zmienił.
- **Rdzeń rurocągu nie mô zaleznoscë òd trzecëch firmë** — mòdułë parsowaniô / manifeztu / delty dzejają ë testëją sã na czëstim Pythonie bez niczégò zainstalowónégò z pip; etapë OCR/dolmaczeniô grackò sã degredują, czedë fakultatiwnëch paczétów felëje.
- **Strona na krawãdzy** stosëje scësłé nagłówczi bezpiékańsczé + CSP (bez `unsafe-inline`; wlënkòwi JSON-LD z pinã sha256), negòcjacëjã jãzyka przez `Accept-Language` + mapòwanié państwów, 30-dniową pòdrãczną pamiãc stronë w KV ë codniowé pòrządczi cron.
- **Přirôstowé aktualizacëje:** detektór deltë pòrównëje zdrzódłowi indeks ë przekôzywô do rurocągu blós zjinaczi.

### Dlô programistów

Publiczny API na https://www.ufolens.com/api/v1 zwrôcô dokùmentë ë metadané jakno JSON. Anonimòwi przistãp je ògrańczony; pòprosë ò klucz dlô badérów/programistów. Zdrzë sã sekcëjã API na starnie, żebë pòznac kùńcówczi ë limitë.

### Status

Kòd skùńczony; strona wëstawionô na https://www.ufolens.com. Produkcyjno baza danych je wëpełnianô przez ùruchòmienié offline'owégò rurocągu ë pùblikacëjã paczczi do przodku (`cli_publish run --remote`). Pełnô dokùmentacëjô projektowô je w `docs/20260511/`.

### Licencjô / ògrańczenia

- Zdrzódłowé dokùmentë: robòtë federalnégò rządu USA, w publiczny domenë w USA.
- Kòd ti platfòrmë: zdrzë `LICENSE`.
- Strona wëséłô `Tdm-Reservation: 1` ë `X-Robots-Tag: noai, noimageai` — indeksowalnô przez szëkôrzë, wëłączonô z trenérowaniô/skrapòwaniô AI.
- Wideòmateriałë są przëpisóné do DVIDS / AARO ë nen projekt nie roscy do nich prôw.

Problemë ë PRë mile widzóné. Prôszã przeczytac `CLAUDE.md` ë `docs/20260511/00-*` przed wnieseniém strukturalnëch zjinaków.

