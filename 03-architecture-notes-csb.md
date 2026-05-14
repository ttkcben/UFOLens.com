# GitHub — Póstrzédny pòst 3 z 3 · Notatczi architektoniczné (Diskùsëjô w sztélu ADR)

**Ùżëj jakno:** diskùsëją w sekcëji "Pòkôż ë òpòwiédz" / "Architektura" abò jakno zaczątk do ADR w `docs/`.
**Klëczowé słowa:** architektura, ADR, automat stanów blós do przodku, môlowy LLM, Ollama, OCR, edge computing, CSP, nagłówczi bezpiékańsczé, rurocąg danych, inżynieria kòsztów, manifezt SQLite, D1, R2, KV
**Hiperłącza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Dlôcze ufolens.com je tak zbudowóné

Notatczi na téma trzech decyzëji, chtërne ùksztôłtowałë [ufolens.com](https://www.ufolens.com) (przeszëkiwalné, wielojãzyczné przebùdowanié [archiwùm PURSUE UAP](https://www.war.gov/ufo)). Kòmentarzë / sprzecëw mile widzóné.

### 1. Rurocąg to automat stanów blós do przodku — z rozmysłã

Stanë: `òdkrëté → pòbróné → ocr_zrobioné → zrobioné_dolmaczenié → opùblikòwóné`. Dokùment przesuwo sã blós do przodku ë blós czedë je co do zrobieniô. Pùblikòwónô zawartosc nigdë nie je znowù przerôbianô, chëbà że detektór deltë pòstrzegnie, że zdrój naprawdã sã zjinacził.

**Dlôcze:** OCR + dolmaczenié to drodżé òperacëje, a archiwùm rosnie z czasã. Rurocąg, co "wszëtkò znowù przerôbiô, żebë bëc pewnym", mô nieògrańczoné kòsztë. Zrobienié niemożlëwim przechòdów w stół sprawiô, że niekòntrolowóny rachùnk je niemożlëwi. Stróp kòsztów to własnosc grafu stanów, a nie czujnoscë òperatora.

**Kòszt:** migracëje schemë ë ponowioné przerôbianié na żëczenié są specjalno cãżczé. Akceptowalny kòmpromis.

### 2. OCR ë dolmaczenié dzejają na môlowim LLM, a nie na chmùrowim API

OCR: òtemkłi motor, alternatiwa to Tesseract CLI. Dolmaczenié + NER: Gemma przez Ollama, na laptopie Apple Silicon.

**Dlôcze:** zerowi kòszt marginalny na dokùment; pòwtôrzalnosc (stałi mòdel + promptë); ë etap pòbiéraniô ju mùszi dzejac z domôcégò IP (zdrój je za Akamai Bot Manager — `curl` dostôwô 403), więc laptop i tak je w grze.

**Kòszt:** jakosc dolmaczeniô je niższô niż w nônowszych mòdelach. Dlô referencëjnégò kòrpusu, gdze òriginalny anielsczi tekst je zawdë na klikniãcé, to je w pòrządkù. Nie twierdzimë, że dolmaczenia są autorytatiwné.

### 3. Te dwie pòłowë mają dokładno jeden interfejs: pùblikòwóną paczkã

Rurocąg nigdë nie zapisëje prosto do produkcyjny bazë danych. Wëdôwô `{ SQL, manifezt aktywów, lësta do czëszczeniô pòdrãczny pamiãcë }`. "Pùblikòwanié" = zastosowanié ti paczczi do przodku (wëpchniãcé SQL do krawãdzowi bazë SQL, zsynchronizowanié aktywów do magazynu òbiektów, wëczëszczenié nazwónëch kluczów pòdrãczny pamiãcë).

**Dlôcze:** môlowô strona ë krawãdzowô strona mògą sã rozwijac niezależnie; paczkã mòżna zrecenzowac; ë "wdróż dané" mô zawdë ten sóm ksztôłt. Worker to môłô aplikacëjô TypeScript/Hono — scësłé CSP (bez `unsafe-inline`; wlënkòwi JSON-LD z pinã sha256), negòcjacëjô `Accept-Language` + kraj→jãzyk, 30-dniowô pòdrãcznô pamiãc stronë w KV, codniowé pòrządczi cron — ë nigdë nie mùszi wiedzec, jak dané pòwstałë.

**Kòszt:** zjinaczenié schemë D1 dotikô dwóch lopków (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Tanié zabezpieczenié.

### Nie negòcjowalne zasadë wpisóné w dzejanié

- Nie je pòłączoné z rządem USA; żôdnëch òficjalnëch insygniów.
- Zdrzódłowé redakcëje są zachòwóné, nigdë nie òdwrôcané.
- Wideò przëpisóné do DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na całi starnie — indeksowalnô przez szëkôrzë, wëłączonô z trenérowaniô AI.

Na żëwò: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
