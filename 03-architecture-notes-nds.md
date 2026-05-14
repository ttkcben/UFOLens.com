# GitHub — Bidrag 3 vun 3 · Architektur-Anteken (Diskusschoon in’n ADR-Stil)

**Bruken as:** en Diskusschoon ünner "Wiesen un Vertellen" / "Architektur", oder as Grundlaag för `docs/` ADR.
**Slötelwöör:** Architektur, ADR, Vörwärts-Tostandsmaschien, lokaal LLM, Ollama, OCR, Edge Computing, CSP, Sekerheits-Headers, Datenpipeline, Kosten-Engineering, SQLite-Manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Worüm ufolens.com so boot is, as dat is

Anteken to de dree Entscheden, de [ufolens.com](https://www.ufolens.com) (de dörsökbore, mehrsprakige Neeboot vun dat [PURSUE UAP-Archiv](https://www.war.gov/ufo)) präägt hebbt. Kommentaren / Gegenwind sünd willkamen.

### 1. De Pipeline is en Vörwärts-Tostandsmaschien — mit Afsicht

Tostännen: `opdeckt → rünnerladen → ocr_fardig → översett → publizeert`. En Dokument bewegt sik blots vörwärts, un blots, wenn wat to doon is. Publizeert Inholt warrt nienich wedder bearbeidt, wenn nich en Delta-Detektor süht, dat de Born sik tatsächlich ännert hett.

**Worüm:** OCR + Översetten sünd de düren Operatschonen, un dat Archiv wasst mit de Tied. En Pipeline, de "allens wedder utföhrt, üm seker to gahn", hett unbegrenzte Kosten. Dat Unmööglichmaken vun Trüggwärts-Övergäng maakt en ut de Reeg lopen Reken unmööglich. De Kosten-Bövergrenz is en Egenschopp vun den Tostandsgraphen, nich vun de Wachsamkeit vun den Bedriever.

**Kosten:** Schema-Migratschonen un afsichtlich Wedderbearbeiden sünd mit Afsicht unbequem. En akzepteerbor Kompromiss.

### 2. OCR un Översetten loopt op en lokalen LLM, nich op en Cloud-API

OCR: Open-Source-Engine, Tesseract CLI Fallback. Översetten + NER: Gemma över Ollama, op en Apple Silicon Laptop.

**Worüm:** keen marginalen Kosten pro Dokument; reproduzeerbor (fast Modell + Prompts); un de Fetch-Schritt mutt al vun en privaten IP-Adress utföhrt warrn (de Born is achter Akamai Bot Manager — `curl` kriggt en 403), also is en Laptop sowieso in de Sliep.

**Kosten:** de Översettensqualität is ünner en Spitzenmodell. För en Referenzkorpus, woneem dat engelsche Original jümmers een Klick weg is, is dat in Ornen. Wi seggt nich, dat de Översetten autoritativ sünd.

### 3. De twee Halvdelen deelt nipp un nau een Schnittsteed: en publizeert Bünnel

De Pipeline schrifft nienich direkt in de Produktschonsdatenbank. Se gifft `{ SQL, Asset-Manifest, Cache-Purge-List }` ut. "Publizeren" = dit Bünnel vörwärts anwennen (SQL na de Edge SQL DB pushen, Assets mit den Objektspieker synchroniseren, de nöömten Cache-Slötels leddig maken).

**Worüm:** de lokale Siet un de Edge-Siet köönt sik unafhängig entwickeln; dat Bünnel is to överprüfen; un "Daten insetten" hett jümmers de sülve Form. De Worker is en lütte TypeScript/Hono-App — strikte CSP (keen `unsafe-inline`; Inline-JSON-LD is mit sha256 fastpVolkswagen), `Accept-Language` + Länner→Spraak-Verhanneln, 30-Daag-KV-Sietencache, daaglichen Housekeeping-Cron — un he mutt nienich weten, wo de Daten maakt worrn sünd.

**Kosten:** en D1-Schema-Ännern raakt twee Datein (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). En billige Versekerung.

### Nich-verhannelbore Saken, de in’t Verhollen inbackt sünd

- Nich mit de U.S.-Regeren verbunnen; keen offiziellen Insignien.
- Born-Redaktschonen warrt bewohrt, nienich trüggängig maakt.
- Video warrt DVIDS / AARO toschreven.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` över de hele Siet — söök-indexeerbor, AI-Scrape-utslooten.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

