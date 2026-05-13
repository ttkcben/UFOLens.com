# GitHub — Beitrag 3 von 3 · Architektur-Notizen (Discussion im ADR-Stil)

**Verwendung:** eine Discussion unter "Show and tell" / "Architecture" oder ein `docs/`-ADR-Keim.
**Stichworte:** Architektur, ADR, Forward-only-Zustandsmaschine, lokales LLM, Ollama, OCR, Edge-Computing, CSP, Security-Header, Datenpipeline, Kosten-Engineering, SQLite-Manifest, D1, R2, KV
**Links:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Warum ufolens.com so gebaut ist, wie es ist

Notizen zu den drei Entscheidungen, die [ufolens.com](https://www.ufolens.com) (den durchsuchbaren, mehrsprachigen Neuaufbau des [PURSUE-UAP-Archivs](https://www.war.gov/ufo)) geformt haben. Kommentare / Gegenpositionen sind willkommen.

### 1. Die Pipeline ist absichtlich eine Forward-only-Zustandsmaschine

Zustände: `discovered → downloaded → ocr_done → translated → published`. Ein Dokument bewegt sich nur vorwärts — und nur, wenn es etwas zu tun gibt. Publizierte Inhalte werden nie neu verarbeitet, es sei denn, ein Delta-Detector stellt fest, dass sich die Quelle tatsächlich geändert hat.

**Warum:** OCR + Übersetzung sind die teuren Operationen, und das Archiv wächst mit der Zeit. Eine Pipeline, die "vorsichtshalber alles neu laufen lässt", hat unbegrenzte Kosten. Rückwärtsübergänge strukturell unmöglich zu machen, macht eine außer Kontrolle geratene Rechnung unmöglich. Die Kostendeckelung ist eine Eigenschaft des Zustandsgraphen — und nicht der Wachsamkeit der Betreiber.

**Preis:** Schema-Migrationen und absichtliches Reprocessing sind bewusst unbequem. Akzeptabler Trade-off.

### 2. OCR und Übersetzung laufen auf einem lokalen LLM, nicht auf einer Cloud-API

OCR: Open-Source-Engine, Tesseract CLI als Fallback. Übersetzung + NER: Gemma via Ollama, auf einem Apple-Silicon-Laptop.

**Warum:** null Grenzkosten pro Dokument; reproduzierbar (fixiertes Modell + Prompts); und der Fetch-Schritt muss ohnehin von einer Heim-IP aus laufen (die Quelle sitzt hinter Akamai Bot Manager — `curl` bekommt 403), der Laptop steckt also sowieso schon im Loop.

**Preis:** die Übersetzungsqualität ist unterhalb eines Frontier-Modells. Für ein Referenzkorpus, in dem das englische Original immer einen Klick entfernt ist, ist das in Ordnung. Wir beanspruchen nicht, dass die Übersetzungen autoritativ sind.

### 3. Die beiden Hälften teilen genau eine Schnittstelle: das publizierte Bundle

Die Pipeline schreibt niemals direkt in die Produktionsdatenbank. Sie liefert `{ SQL, Asset-Manifest, Cache-Purge-Liste }`. "Publizieren" = dieses Bundle vorwärts anwenden (SQL an die Edge SQL DB pushen, Assets zum Object Storage synchen, die genannten Cache-Keys purgen).

**Warum:** die lokale Seite und die Edge-Seite können sich unabhängig weiterentwickeln; das Bundle ist reviewbar; und "Daten deployen" hat jedes Mal dieselbe Form. Der Worker ist eine kleine TypeScript-/Hono-App — strikte CSP (kein `unsafe-inline`; inline JSON-LD per sha256 gepinnt), `Accept-Language` + Land→Sprache-Verhandlung, 30-Tage-KV-Seiten-Cache, täglicher Housekeeping-Cron — und muss nie wissen, wie die Daten entstanden sind.

**Preis:** eine D1-Schema-Änderung berührt zwei Dateien (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Billige Versicherung.

### Im Verhalten verankerte Nicht-Verhandelbare

- Nicht mit der US-Regierung verbunden; keine offiziellen Hoheitszeichen.
- Schwärzungen der Quelle werden bewahrt, niemals rückgängig gemacht.
- Video wird DVIDS / AARO zugeordnet.
- Site-weit: `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` — indexierbar durch Suchmaschinen, Opt-out gegenüber KI-Scraping.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
