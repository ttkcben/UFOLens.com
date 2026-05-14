# GitHub — Post 3 z 3 · Notatki architektoniczne (dyskusja w stylu ADR)

**Zastosowanie:** jako dyskusja w sekcji "Pokaż i opowiedz" / "Architektura" lub jako zalążek ADR w `docs/`.
**Słowa kluczowe:** architektura, ADR, maszyna stanów tylko do przodu, lokalny LLM, Ollama, OCR, edge computing, CSP, nagłówki bezpieczeństwa, potok danych, inżynieria kosztów, manifest SQLite, D1, R2, KV
**Hiperłącza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Dlaczego ufolens.com jest zbudowany w ten sposób

Notatki na temat trzech decyzji, które ukształtowały [ufolens.com](https://www.ufolens.com) (przeszukiwalną, wielojęzyczną przebudowę [archiwum PURSUE UAP](https://www.war.gov/ufo)). Komentarze / krytyka mile widziane.

### 1. Potok jest celowo maszyną stanów działającą tylko do przodu

Stany: `odkryty → pobrany → ocr_gotowy → przetłumaczony → opublikowany`. Dokument porusza się tylko do przodu i tylko wtedy, gdy jest praca do wykonania. Opublikowana treść nigdy nie jest ponownie przetwarzana, chyba że detektor różnic zauważy, że źródło faktycznie się zmieniło.

**Dlaczego:** OCR + tłumaczenie to najdroższe operacje, a archiwum z czasem rośnie. Potok, który "ponownie uruchamia wszystko dla bezpieczeństwa", ma nieograniczony koszt. Uniemożliwienie przejść wstecznych uniemożliwia niekontrolowany rachunek. Pułap kosztów jest właściwością grafu stanów, a nie czujności operatora.

**Koszt:** migracje schematów i celowe ponowne przetwarzanie są celowo niewygodne. Akceptowalny kompromis.

### 2. OCR i tłumaczenie działają na lokalnym LLM, a nie na chmurowym API

OCR: silnik open-source, awaryjnie Tesseract CLI. Tłumaczenie + NER: Gemma przez Ollama, na laptopie z Apple Silicon.

**Dlaczego:** zerowy koszt krańcowy za dokument; powtarzalność (stały model + prompty); a krok pobierania i tak musi być uruchamiany z domowego adresu IP (źródło jest za Akamai Bot Manager — `curl` dostaje 403), więc laptop i tak jest w obiegu.

**Koszt:** jakość tłumaczenia jest niższa niż w przypadku najnowocześniejszych modeli. Dla korpusu referencyjnego, gdzie oryginalny angielski jest zawsze dostępny jednym kliknięciem, jest to w porządku. Nie twierdzimy, że tłumaczenia są autorytatywne.

### 3. Dwie połówki dzielą dokładnie jeden interfejs: opublikowany pakiet

Potok nigdy nie zapisuje bezpośrednio do produkcyjnej bazy danych. Emituje `{ SQL, manifest zasobów, lista do czyszczenia pamięci podręcznej }`. "Publikowanie" = zastosowanie tego pakietu (wypchnięcie SQL do brzegowej bazy danych SQL, synchronizacja zasobów z magazynem obiektów, wyczyszczenie nazwanych kluczy pamięci podręcznej).

**Dlaczego:** strona lokalna i strona brzegowa mogą ewoluować niezależnie; pakiet można przeglądać; a "wdrażanie danych" ma za każdym razem ten sam kształt. Worker to mała aplikacja TypeScript/Hono — rygorystyczny CSP (bez `unsafe-inline`; wbudowany JSON-LD jest przypięty przez sha256), `Accept-Language` + negocjacja kraj→język, 30-dniowa pamięć podręczna stron w KV, codzienny cron porządkowy — i nigdy nie musi wiedzieć, jak dane zostały utworzone.

**Koszt:** zmiana schematu D1 dotyka dwóch plików (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Tanie ubezpieczenie.

### Nienegocjowalne zasady wbudowane w zachowanie

- Nie jesteśmy powiązani z rządem USA; brak oficjalnych insygniów.
- Redakcje źródłowe są zachowane, nigdy nieodwracane.
- Wideo przypisane do DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na całej stronie — indeksowalna przez wyszukiwarki, z wyłączeniem scrapingu przez AI.

Na żywo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
