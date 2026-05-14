# GitHub — Postitus 3/3 · Arhitektuurimärkmed (ADR-stiilis arutelu)

**Kasutus:** aruteluna jaotises "Näita ja räägi" / "Arhitektuur" või `docs/` ADR-i seemneks.
**Märksõnad:** arhitektuur, ADR, ainult edasiliikuv olekumasin, lokaalne LLM, Ollama, OCR, servatöötlus, CSP, turvapäised, andmetoru, kulude optimeerimine, SQLite manifest, D1, R2, KV
**Hüperlingid:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Miks ufolens.com on ehitatud nii, nagu ta on

Märkmeid kolme otsuse kohta, mis kujundasid [ufolens.com-i](https://www.ufolens.com) (otsitav, mitmekeelne [PURSUE UAP arhiivi](https://www.war.gov/ufo) ümberehitus). Kommentaarid / vastuväited on teretulnud.

### 1. Toru on sihilikult ainult edasiliikuv olekumasin

Olekud: `discovered → downloaded → ocr_done → translated → published`. Dokument liigub ainult edasi ja ainult siis, kui on tööd teha. Avaldatud sisu ei töödelda kunagi uuesti, kui delta detektor ei näe, et lähteallikas on tegelikult muutunud.

**Miks:** OCR + tõlkimine on kulukad operatsioonid ja arhiiv kasvab aja jooksul. Torul, mis "turvalisuse huvides kõike uuesti käitab", on piiramatu kulu. Tagurpidi üleminekute võimatuks tegemine muudab kontrolli alt väljunud arve võimatuks. Kululagi on olekugraafiku omadus, mitte operaatori valvsuse küsimus.

**Hind:** skeemimigratsioonid ja sihilik uuesti töötlemine on teadlikult kohmakad. Vastuvõetav kompromiss.

### 2. OCR ja tõlkimine toimuvad lokaalse LLM-iga, mitte pilve API-ga

OCR: avatud lähtekoodiga mootor, Tesseract CLI varuvariant. Tõlkimine + NER: Gemma Ollama kaudu, Apple Silicon sülearvutis.

**Miks:** null piirkulu dokumendi kohta; reprodutseeritav (fikseeritud mudel + promptid); ja hankimisetapp peab juba niikuinii toimuma elukoha IP-lt (lähteallikas on Akamai Bot Manageri taga — `curl` saab 403), seega on sülearvuti igal juhul tsüklis sees.

**Hind:** tõlkekvaliteet on alla tipptasemel mudeli. Viitekorpuse jaoks, kus originaalne ingliskeelne tekst on alati ühe kliki kaugusel, on see vastuvõetav. Me ei väida, et tõlked on autoriteetsed.

### 3. Kahel poolel on täpselt üks liides: avaldatud kimp

Toru ei kirjuta kunagi otse tootmisandmebaasi. See väljastab `{ SQL, varade manifest, vahemälu tühjendamise nimekiri }`. "Avaldamine" = rakenda see kimp edasi (tõuka SQL serva SQL DB-sse, sünkroniseeri varad objektimäluga, tühjenda nimetatud vahemäluvõtmed).

**Miks:** lokaalne pool ja servapool saavad areneda iseseisvalt; kimp on ülevaadatav; ja "andmete kasutuselevõtt" on iga kord sama kujuga. Worker on väike TypeScript/Hono rakendus — range CSP (ei ole `unsafe-inline`; inline JSON-LD on sha256-ga kinnitatud), `Accept-Language` + riik→keel läbirääkimised, 30-päevane KV lehe vahemälu, igapäevane korrastustöö — ja see ei pea kunagi teadma, kuidas andmed loodi.

**Hind:** D1 skeemi muudatus puudutab kahte faili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Odav kindlustus.

### Käitumisse sisse ehitatud läbirääkimatud punktid

- Ei ole seotud USA valitsusega; ei mingeid ametlikke sümboleid.
- Lähteallika redigeerimised säilitatakse, neid ei tühistata kunagi.
- Video on omistatud DVIDS / AARO-le.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` kogu saidil — otsinguindekseeritav, AI kraapimisest on loobutud.

Veebisait: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
