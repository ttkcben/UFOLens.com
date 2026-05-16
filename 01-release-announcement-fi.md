# GitHub – Julkaisu 1/3 · Vapautus- / README-ilmoitus

**Käyttö:** GitHub-julkaisun tekstinä, kiinnitettynä keskusteluna tai repon README-tiedoston yläosassa.
**Avainsanat:** UAP, UFO, PURSUE-arkisto, salassapidosta vapautetut asiakirjat, avoin data, kokotekstihaku, OCR, konekääntäminen, paikallinen LLM, Ollama, reunalaskenta, julkinen API, Hono, TypeScript, Python
**Hyperlinkit:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com – monikielinen, haettava alusta PURSUE UAP -arkistolle

**Livenä:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Lähdearkisto:** https://www.war.gov/ufo

`ufolens.com` uudelleenjulklaisee Yhdysvaltain sotaministeriön **PURSUE**-arkiston salassapidosta vapautettuja UAP-/UFO-tietueita tietoalustana: kokotekstihaku, konekäännökset koko aineistossa, kartta- ja aikajanatutkimus sekä julkinen JSON API. Lähdeasiakirjat ovat Yhdysvaltain liittohallituksen teoksia ja Yhdysvalloissa ne ovat public domainia ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Tämä projekti **ei ole sidoksissa Yhdysvaltain hallitukseen**, ei käytä virallisia tunnuksia eikä koskaan kumoa toimituksellisia poistoja.

### Arkkitehtuuri

```
Paikallinen kone (Apple Silicon, kotiverkon IP)      Reunaverkko
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, vain stdlib-ydin)           worker/  (TypeScript, Hono.js)
  nouda → OCR → käännä → julkaise (vain eteenpäin)      /{lang}/...   sivut
  OCR: avoimen lähdekoodin moottori (Tesseract CLI varalla) /api/v1/...   julkinen API
  käännös / NER: paikallinen LLM (Gemma Ollaman kautta)     /admin        operaattorin konsoli
  tila: SQLite-manifesti                             taustalla: reunan SQL-tietokanta, objektitallennus
        │                                              (lähde-PDF:t), KV-välimuisti
        └── julkaisee paketin: SQL + resurssimanifesti + välimuistin tyhjennyslista ──┘
```

- **Nolla pilvi-AI-kustannusta per asiakirja.** OCR ja kääntäminen ajetaan paikallisesti; vain eteenpäin suuntautuva tilakone (`discovered → downloaded → ocr_done → translated → published`) takaa, ettei asiakirjaa käsitellä uudelleen, ellei se ole muuttunut.
- **Putken ytimellä ei ole kolmannen osapuolen riippuvuuksia** — jäsennys-, manifesti- ja delta-moduulit toimivat ja testataan puhtaalla Pythonilla ilman pip-asennuksia; OCR-/käännösvaiheet toimivat rajoitetusti, kun valinnaiset paketit puuttuvat.
- **Reunasivusto** käyttää tiukkoja turvallisuusotsakkeita + CSP:tä (ei `unsafe-inline`; inline JSON-LD on sha256-kiinnitetty), kielineuvottelua `Accept-Language`-otsakkeen + maakohtaisen kartoituksen kautta, 30 päivän KV-sivuvälimuistia ja päivittäistä ylläpito-cron-työtä.
- **Inkrementaaliset päivitykset:** delta-tunnistin vertaa lähdeindeksiä ja syöttää vain muutokset takaisin putkeen.

### Kehittäjille

Julkinen API osoitteessa https://www.ufolens.com/api/v1 palauttaa asiakirjoja ja metadataa JSON-muodossa. Anonyymi pääsy on rajoitettu; pyydä avain tutkija-/kehittäjätasoille. Katso sivuston API-osiosta päätepisteet ja rajat.

### Tila

Koodi valmis; sivusto otettu käyttöön osoitteessa https://www.ufolens.com. Tuotantotietokanta täytetään ajamalla offline-putki ja julkaisemalla paketti eteenpäin (`cli_publish run --remote`). Täydelliset suunnitteluasiakirjat löytyvät kansiosta `docs/20260511/`.

### Lisenssi / rajaukset

- Lähdeasiakirjat: Yhdysvaltain liittohallituksen teoksia, public domain Yhdysvalloissa.
- Tämän alustan oma koodi: katso `LICENSE`.
- Sivusto lähettää otsakkeet `Tdm-Reservation: 1` ja `X-Robots-Tag: noai, noimageai` — hakukoneiden indeksoitavissa, mutta tekoälykoulutuksesta/kaapimisesta estetty.
- Videomateriaali on DVIDS:n / AARO:n tuottamaa, eikä tämä projekti vaadi siihen oikeuksia.

Issue-ilmoitukset ja PR:t ovat tervetulleita. Lue `CLAUDE.md` ja `docs/20260511/00-*` ennen rakenteellisten muutosten avaamista.
