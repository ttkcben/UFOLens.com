# GitHub — Beitrag 3 vo 3 · Architekturnotizn (Diskussion im ADR-Stil)

**Vawendung:** Ois a Diskussion unta "Zeign und erklärn" / "Architektur", oder ois Startpunkt fia a `docs/` ADR.
**Schlisslwörter:** Architektur, ADR, forward-only Zustandsmaschin, lokals LLM, Ollama, OCR, Edge Computing, CSP, Sicherheits-Header, Datnpipeline, Kostn-Engineering, SQLite-Manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Warum ufolens.com so baut is, wia's is

Notizn zu de drei Entscheidunga, de [ufolens.com](https://www.ufolens.com) (de suachbore, mehrsprochige Nei-Version vom [PURSUE UAP-Archiv](https://www.war.gov/ufo)) geprägt hom. Kommentare / Gegnwind san willkomma.

### 1. De Pipeline is a "forward-only" Zustandsmaschin — mit Obsicht

Zuständ: `discovered → downloaded → ocr_done → translated → published`. A Dokument bewegt si nua vorwärts, und a nua, wenn's Orbeit gibt. A vaeffentlichter Inhoid werd nia nei vaoarbeit, außer a Delta-Detektor merkt, dass si de Quelln wirklich g'ändert hot.

**Warum:** OCR + Ibasetzung san de teian Operationa, und s'Archiv wachst mit da Zeit. A Pipeline, de "ois nei mocht, um auf Nummer sicha z'geh", hot unbegrenzte Kostn. Wenn ma Rückwärtsschritte unmeglich mocht, mocht ma a an Kostn-Amoklauf unmeglich. Da Kostndeckl is a Eigenschaft vom Zustandsgraph, ned vo da Wachsamkeit vom Betreiba.

**Da Preis:** Schema-Migrationa und absichtliche Neivaoabeitung san extra umständlich. A akzeptierbora Kompromiss.

### 2. OCR und Ibasetzung laffn auf am lokaln LLM, ned auf am Cloud-API

OCR: Open-Source-Engine, Tesseract CLI Fallback. Ibasetzung + NER: Gemma iba Ollama, auf am Apple Silicon Laptop.

**Warum:** Nui Grenzkostn pro Dokument; reproduzierbor (fixs Modell + Prompts); und da Abruafschritt muass eh vo ana privatn IP laffn (de Quelln is hinta Akamai Bot Manager — `curl` kriagt an 403er), oiso is a Laptop eh scho im Spui.

**Da Preis:** de Ibasetzungsqualität is schlechta ois bei am Top-Modell. Fia an Referenzkorpus, wo ma mit oam Klick zum englischn Original kimmt, is des in Ordnung. Mia behauptn ned, dass de Ibasetzunga verbindlich san.

### 3. De zwoa Hälftn teiln si genau oa Schnittstej: a vaeffentlichts Bündl

De Pipeline schreibt nia direkt in de Produktionsdatnbank. Sie spuckt `{ SQL, Asset-Manifest, Cache-Löschlistn }` aus. "Vaeffentlichn" = des Bündl anwendn (SQL in de Edge-SQL-DB pushen, Assets mitm Objektspeicha synchronisian, de gnanntn Cache-Schlissl löschn).

**Warum:** de lokale Seitn und de Edge-Seitn kenna si unabhängig weitaentwickln; s'Bündl ko ma si onschaun; und "Datn bereitstelln" hot jeds Moi de gleiche Form. Da Worker is a kloane TypeScript/Hono-App — strikts CSP (koa `unsafe-inline`; inline JSON-LD is mit sha256-pinnt), `Accept-Language` + Land→Sproch-Vahandlung, 30-Tog-KV-Seitn-Cache, täglicha Wartungs-Cron — und er muass nia wissn, wia de Datn entstandn san.

**Da Preis:** a D1-Schema-Änderung betrifft zwoa Datein (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). A billige Vasicherung.

### Unvahandelbore Prinzipien, de im Vahoidn vabaut san

- Ned mit da US-Regiarung vabundn; koane offizielln Obzeichn.
- Quelln-Schwärzunga bleibn, wia's san, und wern nia rückgängig gmocht.
- Videos wern DVIDS / AARO zuagschriebn.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` auf da ganzn Seitn — suach-indexierbor, vom KI-Scraping ausgnumma.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
