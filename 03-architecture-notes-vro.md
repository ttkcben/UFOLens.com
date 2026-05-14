# GitHub — Postitus 3/3 · Arhitektuuri märkmed (ADR-stiilis arutelu)

**Kasuta nigu:** arutelu jaotusen "Näütäq ja kõnõlõ" / "Arhitektuur" vai `docs/` ADR-i alus.
**Märksõnaq:** arhitektuur, ADR, õnnõ edespidine seisundimassin, paigapäälne LLM, Ollama, OCR, servaarvutus, CSP, turvapäised, andmetorustik, kulude projekteeriminõ, SQLite manifest, D1, R2, KV
**Hüperlingiq:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Minkperäst ufolens.com om ehitet nii, nigu tä om

Märkmed kolmõ otsussõ kotsilõ, miä kujondiq [ufolens.com](https://www.ufolens.com)-i (otsitav, mitmõkeeline [PURSUE UAP arhiivi](https://www.war.gov/ufo) ümberehitus). Kommentaariq / vastuväiteq ommaq teretulnuq.

### 1. Torujuhe om õnnõ edespidine seisundimassin — meelega

Seisundiq: `discovered → downloaded → ocr_done → translated → published`. Dokument liigus õnnõ edesi ja õnnõ sis, ku om tüüd tetäq. Avaldõt sisu ei tüüdeldäq kunagi vahtsõst, ku delta-detektor ei näeq, et lätt om tegelikult muutunuq.

**Minkperäst:** OCR + tõlkminõ ommaq kalliq operatsiooniq ja arhiiv kasus aoga. Torujuhtmõl, miä "kõik vahtsõst läbi käivitäs, et kindel olla", om piiramatu kulu. Tagasi liikumise võimatuks tegemine tege võimatuks ka kontrollimatu arvõ. Kulude ülemmäär om seisundigraafiku, mitte operaatori valvsusõ umahus.

**Kulu:** skeemi migratsiooniq ja meelega vahtsõst töötlemine ommaq tahtlikult kohmakad. Vastu võetav kompromiss.

### 2. OCR ja tõlkminõ tüütäseq paigapäälse LLM-iga, mitte pilve-API-ga

OCR: avaligu lättetekstiga mootor, Tesseract CLI varu. Tõlkminõ + NER: Gemma Ollama kaudu, Apple Siliconi sülearvutin.

**Minkperäst:** null marginaalne kulu dokumendi kotsilõ; reprodutseeritav (fikseerit mudel + vihjeq); ja toomisõ samm piät niikuinii tüütämä elamuvõrgo IP-lt (lätt om Akamai Bot Manageri takan — `curl` saa 403), nii et sülearvuti om niikuinii ringin.

**Kulu:** tõlkekvaliteet om allpool piirimudelit. Viitekorpusõ jaos, kon originaal inglisekeelne tekst om alati üte kliki kaugusel, om tuu hää. Mi ei väidaq, et tõlkõq ommaq autoriteetsed.

### 3. Katõl poolõl om täpselt üts liides: avaldõt komplekt

Torujuhe ei kirodaq kunagi otse tuutmisandmõbaasi. Tuu välästäs `{ SQL, varade manifest, vahemälu tühjendamise nimekiri }`. "Avaldamine" = rakendaq tuu komplekt edesi (lükäq SQL serva SQL DB-he, sünkroniseeriq varaobjekti hoiusõga, tühistäq nimetatud vahemälu võtmed).

**Minkperäst:** paigapäälne külg ja servakülg võivaq arenedaq eraldi; komplekt om ülevaadatav; ja "andmete kasutuselevõtt" om egal kõrral sama kujuga. Worker om väike TypeScript/Hono rakendus — range CSP (olõ-iq `unsafe-inline`; ridades JSON-LD om sha256-ga kinnütet), `Accept-Language` + riigi→keele läbirääkimine, 30-päeväne KV lehe vahemälu, egäpääväne majapidamis-cron — ja tä ei piäq kunagi teadma, kuis andmõq ommaq tettüq.

**Kulu:** D1 skeemi muutmine pututas katõ faili (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Odav kindlustus.

### Käitumisõ sisse ehitet vältimätüq as'aq

- Olõ-iq köüdet USA valitsusõga; olõ-iq ammõtliidsi tunnismärke.
- Lähte redaktsiooniq säilitetäs, kunagi ei pööraq tagasi.
- Video om perit DVIDS / AARO-st.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` terve saidi jaos — otsingumootorilõ indekseeritäv, AI kraapimisõst vällä jäet.

Otseülekanne: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

