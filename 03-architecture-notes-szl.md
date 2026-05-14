# GitHub — Post 3 z 3 · Notatki architektoniczne (Dyskusja w stylu ADR)

**Użyj jako:** dyskusja w "Pokaż i opowiedz" / "Architektura" lub jako zalążek ADR w `docs/`.
**Słowa kluczowe:** architektura, ADR, maszyna stanu tylko do przodu, lokalny LLM, Ollama, OCR, przetwarzanie brzegowe, CSP, nagłówki bezpieczeństwa, potok danych, inżynieria kosztów, manifest SQLite, D1, R2, KV
**Hiperłącza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Czamu ufolens.com jest tak zbudowane, jak jest

Notatki o trzech decyzjach, kere ukształtowały [ufolens.com](https://www.ufolens.com) (przeszukiwalnõ, wielojęzycznõ przebudowã archiwum [PURSUE UAP](https://www.war.gov/ufo)). Komentarze / krytyka mile widziane.

### 1. Pipeline to maszyna stanu ino do przodku — celowo

Stany: `discovered → downloaded → ocr_done → translated → published`. Dokument poruszo sie ino do przodku i ino wtedy, kej jest robota do zrobienia. Opublikowano treść nigdy niy jest przetwarzano na nowo, chyba że detektor delta zauważy, że źrōdło sie faktycznie zmiyniło.

**Dlaczego:** OCR + tłumaczenie to drogie operacje, a archiwum z czasym rośnie. Pipeline, kery "przerabia wszystko na wszelki wypadek", mo nieograniczone koszty. Uniemożliwienie kroków wstecz uniemożliwia niekontrolowany rachunek. Sufit kosztów to właściwość grafu stanu, a nie czujności operatora.

**Koszt:** migracje schematów i celowe ponowne przetwarzanie sōm świadomie niewygodne. Akceptowalny kompromis.

### 2. OCR i tłumaczenie działajōm na lokalnym LLM, a nie na chmurowym API

OCR: silnik open-source, fallback na Tesseract CLI. Tłumaczenie + NER: Gemma przez Ollama, na laptopie Apple Silicon.

**Dlaczego:** zero krańcowego kosztu na dokument; powtarzalność (stały model + prompty); a krok pobierania i tak musi działać z domowego adresu IP (źrōdło jest za Akamai Bot Manager — `curl` dostaje 403), więc laptop i tak jest w obiegu.

**Koszt:** jakość tłumaczenia jest niższa niż w przypadku modelu granicznego. Dla korpusu referencyjnego, gdzie oryginalny angielski jest zawsze o jedno kliknięcie, to jest w porządku. Nie twierdzimy, że tłumaczenia są autorytatywne.

### 3. Dwie połówki majōm dokładnie jeden interfejs: opublikowany pakiet

Pipeline nigdy nie pisze bezpośrednio do produkcyjnej bazy danych. Emituje `{ SQL, manifest zasobów, lista do czyszczenia cache'u }`. "Publikowanie" = zastosowanie tego pakietu do przodu (wgranie SQL do brzegowej bazy SQL, synchronizacja zasobów z przechowalnią obiektów, wyczyszczenie nazwanych kluczy cache'u).

**Dlaczego:** strona lokalna i brzegowa mogą ewoluować niezależnie; pakiet można przejrzeć; a "wdrożenie danych" ma za każdym razem ten sam kształt. Worker to mała aplikacja TypeScript/Hono — rygorystyczne CSP (bez `unsafe-inline`; wbudowany JSON-LD jest przypięty sha256), negocjacja `Accept-Language` + kraj→język, 30-dniowy cache stron KV, codzienny cron porządkowy — i nigdy nie musi wiedzieć, jak dane powstały.

**Koszt:** zmiana schematu D1 dotyczy dwóch plików (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Tanie ubezpieczenie.

### Niezmienne zasady wbudowane w działanie

- Niy jest powiązany z rządem USA; żadnych oficjalnych insygniów.
- Redakcje źrōdłowe sōm zachowane, nigdy niyodwracane.
- Wideo przypisane do DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na całej stronie — indeksowalny przez wyszukiwarki, wycofany ze skrobania przez AI.

Na żywo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
