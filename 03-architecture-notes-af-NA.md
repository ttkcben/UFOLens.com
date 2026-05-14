# GitHub — Pos 3 van 3 · Argitektuurnotas (ADR-styl Bespreking)

**Gebruik as:** 'n Bespreking onder "Wys en vertel" / "Argitektuur", of `docs/` ADR-saad.
**Sleutelwoorde:** argitektuur, ADR, slegs-vorentoe-toestandmasjien, plaaslike LLM, Ollama, OCR, randnetwerkrekenaarkunde, CSP, sekuriteitsheaders, datapyplyn, koste-ingenieurswese, SQLite-manifes, D1, R2, KV
**Hipskakels:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Waarom ufolens.com gebou is soos dit is

Aantekeninge oor die drie besluite wat [ufolens.com](https://www.ufolens.com) (die soekbare, meertalige herbouing van die [PURSUE UAP-argief](https://www.war.gov/ufo)) gevorm het. Kommentaar / terugvoer is welkom.

### 1. Die pyplyn is 'n slegs-vorentoe-toestandmasjien — met 'n doel

Toestande: `ontdek → afgelaai → ocr_gedoen → vertaal → gepubliseer`. 'n Dokument beweeg slegs vorentoe, en slegs wanneer daar werk is om te doen. Gepubliseerde inhoud word nooit herverwerk tensy 'n delta-opspoorder sien dat die bron werklik verander het nie.

**Waarom:** OCR + vertaling is die duur operasies, en die argief groei met verloop van tyd. 'n Pyplyn wat "alles herloop om veilig te wees" het onbeperkte koste. Om terugwaartse oorgange onmoontlik te maak, maak 'n wegholrekening onmoontlik. Die kosteplafon is 'n eienskap van die toestandsgrafiek, nie van operateurswagtheid nie.

**Koste:** skema-migrasies en doelbewuste herverwerking is doelbewus lomp. Aanvaarbare kompromie.

### 2. OCR en vertaling loop op 'n plaaslike LLM, nie 'n wolk-API nie

OCR: oopbron-enjin, Tesseract CLI-terugval. Vertaling + NER: Gemma via Ollama, op 'n Apple Silicon-skootrekenaar.

**Waarom:** nul marginale koste per dokument; reproduseerbaar (vaste model + aansporings); en die haal-stap moet reeds vanaf 'n residensiële IP loop (die bron is agter Akamai Bot Manager — `curl` kry 'n 403), so 'n skootrekenaar is in elk geval in die kringloop.

**Koste:** vertalingskwaliteit is laer as dié van 'n grensmodel. Vir 'n verwysingskorpus waar die oorspronklike Engels altyd net een kliek weg is, is dit aanvaarbaar. Ons maak nie daarop aanspraak dat die vertalings gesaghebbend is nie.

### 3. Die twee helftes deel presies een koppelvlak: 'n gepubliseerde bundel

Die pyplyn skryf nooit direk na die produksiedatabasis nie. Dit lewer `{ SQL, bate-manifes, kas-skoonmaaklys }`. "Publisering" = pas daardie bundel vorentoe toe (stoot SQL na die rand-SQL-DB, sinkroniseer bates na objekberging, maak die genoemde kas-sleutels skoon).

**Waarom:** die plaaslike kant en die randkant kan onafhanklik ontwikkel; die bundel is hersienbaar; en "ontplooi data" het elke keer dieselfde vorm. Die Worker is 'n klein TypeScript/Hono-toepassing — streng CSP (geen `unsafe-inline`; inlyn JSON-LD is sha256-vasgepen), `Accept-Language` + land→taal-onderhandeling, 30-dae KV-bladkas, daaglikse skoonmaak-cron — en dit hoef nooit te weet hoe die data gemaak is nie.

**Koste:** 'n D1-skemaverandering raak twee lêers (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Goedkoop versekering.

### Nie-onderhandelbare punte wat in gedrag ingebak is

- Nie geaffilieer met die V.S. regering nie; geen amptelike insignia nie.
- Bronredaksies word bewaar, nooit omgekeer nie.
- Video toegeskryf aan DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` oor die hele webwerf — soek-indekseerbaar, onttrek van KI-skraping.

Regstreeks: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

