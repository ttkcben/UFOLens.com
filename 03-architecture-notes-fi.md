# GitHub – Julkaisu 3/3 · Arkkitehtuurihuomioita (ADR-tyylinen keskustelu)

**Käyttö:** Keskusteluna "Esittelyt" / "Arkkitehtuuri" -osiossa, tai `docs/`-kansion ADR-pohjana.
**Avainsanat:** arkkitehtuuri, ADR, vain eteenpäin suuntautuva tilakone, paikallinen LLM, Ollama, OCR, reunalaskenta, CSP, turvallisuusotsakkeet, datankäsittelyputki, kustannussuunnittelu, SQLite-manifesti, D1, R2, KV
**Hyperlinkit:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Miksi ufolens.com on rakennettu niin kuin se on

Huomioita kolmesta päätöksestä, jotka muovasivat [ufolens.com](https://www.ufolens.com) -sivustoa (haettava, monikielinen uudelleenrakennus [PURSUE UAP -arkistosta](https://www.war.gov/ufo)). Kommentit / vasta-argumentit tervetulleita.

### 1. Putki on tarkoituksella vain eteenpäin suuntautuva tilakone

Tilat: `discovered → downloaded → ocr_done → translated → published`. Asiakirja etenee vain eteenpäin, ja vain kun on työtä tehtävänä. Julkaistua sisältöä ei koskaan käsitellä uudelleen, ellei delta-tunnistin huomaa lähteen todella muuttuneen.

**Miksi:** OCR + kääntäminen ovat kalliita operaatioita, ja arkisto kasvaa ajan myötä. Putkella, joka "ajaa kaiken uudelleen varmuuden vuoksi", on rajattomat kustannukset. Taaksepäin suuntautuvien siirtymien tekeminen mahdottomaksi tekee hallitsemattomasta laskusta mahdottoman. Kustannuskatto on tilagraafin ominaisuus, ei operaattorin valppauden.

**Hinta:** skeemamigraatiot ja tarkoituksellinen uudelleenkäsittely ovat tarkoituksella kömpelöitä. Hyväksyttävä kompromissi.

### 2. OCR ja kääntäminen ajetaan paikallisella LLM:llä, ei pilvi-API:lla

OCR: avoimen lähdekoodin moottori, Tesseract CLI varalla. Kääntäminen + NER: Gemma Ollaman kautta, Apple Silicon -kannettavalla.

**Miksi:** nolla marginaalikustannus per asiakirja; toistettavissa (kiinteä malli + kehotteet); ja noutovaihe on joka tapauksessa ajettava kotiverkon IP:stä (lähde on Akamai Bot Managerin takana — `curl` saa 403-virheen), joten kannettava on jo mukana kuviossa.

**Hinta:** käännöksen laatu on heikompi kuin huippumalleilla. Referenssiaineistossa, jossa alkuperäinen englanninkielinen versio on aina yhden napsautuksen päässä, se on hyväksyttävää. Emme väitä käännösten olevan virallisia.

### 3. Kahdella puoliskolla on täsmälleen yksi yhteinen rajapinta: julkaistu paketti

Putki ei koskaan kirjoita suoraan tuotantotietokantaan. Se tuottaa `{ SQL, resurssimanifesti, välimuistin tyhjennyslista }`. "Julkaiseminen" = sovelletaan pakettia eteenpäin (työnnetään SQL reunan SQL-tietokantaan, synkronoidaan resurssit objektitallennukseen, tyhjennetään nimetyt välimuistiavaimet).

**Miksi:** paikallinen puoli ja reunapuoli voivat kehittyä itsenäisesti; paketti on tarkistettavissa; ja "datan käyttöönotto" on joka kerta samanmuotoinen. Worker on pieni TypeScript/Hono-sovellus – tiukka CSP (ei `unsafe-inline`; inline JSON-LD on sha256-kiinnitetty), `Accept-Language` + maa→kieli-neuvottelu, 30 päivän KV-sivuvälimuisti, päivittäinen ylläpito-cron – eikä sen tarvitse koskaan tietää, miten data on luotu.

**Hinta:** D1-skeemamuutos koskettaa kahta tiedostoa (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Halpa vakuutus.

### Käyttäytymiseen sisäänrakennetut ehdottomat säännöt

- Ei yhteyttä Yhdysvaltain hallitukseen; ei virallisia tunnuksia.
- Lähdeasiakirjojen toimitukselliset poistot säilytetään, niitä ei koskaan kumota.
- Videot on merkitty DVIDS:n / AARO:n tuottamiksi.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` koko sivuston laajuinen — hakukoneiden indeksoitavissa, tekoälyn kaapiminen estetty.

Livenä: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
