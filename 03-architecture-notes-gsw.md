# GitHub — Biitrag 3 vo 3 · Architektur-Notize (Diskussion im ADR-Stil)

**Verwendig als:** Diskussion under "Zeig und verzell" / "Architektur", oder als `docs/` ADR-Grundlag.
**Schlüsselwörter:** Architektur, ADR, vorwärtsgerichtet Zustandsmaschiine, lokals LLM, Ollama, OCR, Edge Computing, CSP, Sicherheits-Header, Datepipeline, Choschte-Engineering, SQLite-Manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Warum ufolens.com so baut isch, wie's isch

Notize zu de drei Entscheidige, wo [ufolens.com](https://www.ufolens.com) (de suechbar, mehrschpraachig Neubau vom [PURSUE UAP-Archiv](https://www.war.gov/ufo)) prägt händ. Kommentär / Gägewind willkomme.

### 1. D'Pipeline isch absichtlich e 'forward-only' Zustandsmaschiine

Zueständ: `discovered → downloaded → ocr_done → translated → published`. Es Dokumänt bewegt sich nur vorwärts, und nur wenn's Arbet git. Veröffentlichte Inhalt wird nie wieder verarbeitet, es sei denn, en Delta-Detektor gseht, dass sich d'Quälle tatsächlich g'änderet hät.

**Warum:** OCR + Übersetzig sind di tüüre Operatione, und s'Archiv wachst mit de Ziit. E Pipeline, wo 'alles nomal laufe laht, um sicher z'si', het unbegrenzti Choschte. Indem mer Rückwärtsschritt unmöglich macht, wird e explodierendi Rächnig unmöglich. D'Choschtdeckeni isch e Eigeschaft vom Zustandsgraph, nöd vo de Ufmerksamkeit vom Bediener.

**Choschte:** Schema-Migratione und absichtlichs Neu-Verarbeite sind bewusst umständlich. En akzeptable Kompromiss.

### 2. OCR und Übersetzig laufe uf eme lokale LLM, nöd über e Cloud-API

OCR: Open-Source-Engine, Tesseract CLI Fallback. Übersetzig + NER: Gemma via Ollama, uf eme Apple Silicon Laptop.

**Warum:** Null Grenzkoschte pro Dokumänt; reproduzierbar (fixs Modell + Prompts); und de Fetch-Schritt muess sowieso vo enere Heimet-IP laufe (d'Quälle isch hinder em Akamai Bot Manager – `curl` bechunnt en 403), also isch en Laptop sowieso im Spiel.

**Choschte:** D'Übersetzigsqualität isch unterere-n-eme Frontier-Modell. Für en Referenz-Corpus, wo s'Original-Englisch immer nur en Klick entfernt isch, isch das in Ordnig. Mir behaupted nöd, dass d'Übersetzige verbindlich sind.

### 3. Di beide Hälftene teiled genau ei Schnittstell: es veröffentlichts Bundle

D'Pipeline schriibt nie direkt i d'Produktions-Datebank. Si git `{ SQL, Asset-Manifest, Cache-Löschliste }` us. "Veröffentliche" = das Bundle vorwärts aawende (SQL i d'Edge-SQL-DB pushe, Assets mit em Objektspeicher synchronisiere, di gnennte Cache-Keys lösche).

**Warum:** di lokal Siite und d'Edge-Siite chönd sich unabhängig entwickle; s'Bundle isch überprüefbar; und "Date deploye" isch jedes Mal im gliche Format. De Worker isch e chliini TypeScript/Hono-App — schträngi CSP (keis `unsafe-inline`; inline JSON-LD isch sha256-pinned), `Accept-Language` + Land→Schpraach-Verhandlig, 30-tägige KV-Siite-Cache, tägliche Huushaltigs-Cron — und er muess nie wüsse, wie d'Date gmacht worde sind.

**Choschte:** e D1-Schemaänderig betrifft zwei Dateie (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). E billigi Versicherig.

### Nöd verhandelbari Pünkt, wo im Verhalte verankered sind

- Nöd mit de US-Regierig verbunde; keini offizielle Abzeiche.
- Quälle-Schwärzige blibed erhalte, werded nie rückgängig gmacht.
- Video wird DVIDS / AARO zuegschribe.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` siitewiit — suech-indexierbar, AI-Scrape-Opt-out.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
