# GitHub – Příspěvek 3 ze 3 · Poznámky k architektuře (diskuze ve stylu ADR)

**Použití jako:** diskuze pod „Ukázka a povídání“ / „Architektura“ nebo jako základ pro ADR v `docs/`.
**Klíčová slova:** architektura, ADR, stavový automat pouze pro posun vpřed, lokální LLM, Ollama, OCR, edge computing, CSP, bezpečnostní hlavičky, datová pipeline, cost engineering, manifest v SQLite, D1, R2, KV
**Hypertextové odkazy:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Proč je ufolens.com postaven tak, jak je

Poznámky ke třem rozhodnutím, která formovala [ufolens.com](https://www.ufolens.com) (prohledávatelnou, vícejazyčnou přestavbu archivu [PURSUE UAP](https://www.war.gov/ufo)). Komentáře / kritika jsou vítány.

### 1. Pipeline je záměrně stavový automat pouze pro posun vpřed

Stavy: `objeveno → staženo → ocr_hotovo → přeloženo → publikováno`. Dokument se pohybuje pouze vpřed a pouze tehdy, když je co dělat. Publikovaný obsah není nikdy znovu zpracován, pokud detektor delty nezjistí, že se zdroj skutečně změnil.

**Proč:** OCR + překlad jsou nákladné operace a archiv se časem rozrůstá. Pipeline, která „pro jistotu vše znovu spustí“, má neomezené náklady. Znemožnění zpětných přechodů znemožňuje nekontrolovatelný účet. Strop nákladů je vlastností stavového grafu, nikoli ostražitosti operátora.

**Cena:** migrace schématu a záměrné znovuzpracování jsou schválně nepohodlné. Přijatelný kompromis.

### 2. OCR a překlad běží na lokálním LLM, nikoli na cloudovém API

OCR: open-source engine, Tesseract CLI jako záloha. Překlad + NER: Gemma přes Ollama, na notebooku Apple Silicon.

**Proč:** nulové mezní náklady na dokument; reprodukovatelnost (fixní model + prompty); a krok stahování už stejně musí běžet z rezidenční IP (zdroj je za Akamai Bot Manager – `curl` dostane 403), takže notebook je stejně ve smyčce.

**Cena:** kvalita překladu je nižší než u nejmodernějších modelů. Pro referenční korpus, kde je originální angličtina vždy na jedno kliknutí, je to v pořádku. Netvrdíme, že překlady jsou autoritativní.

### 3. Obě poloviny sdílejí přesně jedno rozhraní: publikovaný balíček

Pipeline nikdy nezapisuje přímo do produkční databáze. Vytváří `{ SQL, manifest aktiv, seznam pro vyčištění cache }`. „Publikování“ = aplikování tohoto balíčku (nahrání SQL do edge SQL DB, synchronizace aktiv do object storage, vyčištění pojmenovaných klíčů v cache).

**Proč:** lokální a edge strana se mohou vyvíjet nezávisle; balíček je přezkoumatelný; a „nasazení dat“ má pokaždé stejný tvar. Worker je malá TypeScript/Hono aplikace – striktní CSP (žádné `unsafe-inline`; inline JSON-LD je připnutý pomocí sha256), `Accept-Language` + vyjednávání země→jazyk, 30denní KV cache stránek, denní údržbový cron – a nikdy nepotřebuje vědět, jak byla data vytvořena.

**Cena:** změna schématu D1 se dotýká dvou souborů (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Levná pojistka.

### Nezpochybnitelná pravidla zakotvená v chování

- Není spojeno s vládou USA; žádné oficiální insignie.
- Zdrojové redakční úpravy jsou zachovány, nikdy ne odstraňovány.
- Video je připisováno DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na celém webu – indexovatelné vyhledávači, odhlášeno ze scrapování pro AI.

Živě: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

