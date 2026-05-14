# GitHub — Dieđáhus 3/3 · Arkitektuvračilgehusat (ADR-staila ságastallan)

**Geavat dego:** ságastallamin "Čájet ja muital" / "Arkitektuvra" vuolde, dahje `docs/` ADR-álgun.
**Čoavddasánit:** arkitektuvra, ADR, ovddosguvlui-dušše stáhtamašiidna, báikkálaš LLM, Ollama, OCR, ravdadihtorprosesseren, CSP, siggervuođabajilčállagat, dáhtápipeline, golluinšenevrabargu, SQLite-manifessta, D1, R2, KV
**Hyperliŋkkat:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Manin ufolens.com lea huksejuvvon ná

Čilgehusat golmma mearrádusa birra mat leat hábmen [ufolens.com](https://www.ufolens.com) (ohcanlágan, máŋggagielat ođđasishuksen [PURSUE UAP-arkiivvas](https://www.war.gov/ufo)). Kommentárat / vuosteháhku buresboahtin.

### 1. Pipeline lea ovddosguvlui-dušše stáhtamašiidna — dáhtolul

Stáhtat: `discovered → downloaded → ocr_done → translated → published`. Áššegirji lihkada dušše ovddosguvlui, ja dušše go lea bargu. Almmuhuvvon sisdoallu ii goassege gieđahallo ođđasit earret go jos delta-fuobmájeaddji oaidná ahte gáldu lea duođaid rievdan.

**Manin:** OCR + jorgaleapmi leat divrras operašuvnnat, ja arkiiva sturru áiggi mielde. Pipeline mii "johtá buot ođđasit sihkkarvuođa dihte" lea ráddjehis gollu. Dahkat maŋosguvlui lihkademiid veadjemeahttumin dahká veadjemeahttumin billahallan goluid. Gollogeahči lea stáhtagráfa iešvuohta, ii geavaheaddji várrogasvuođa.

**Gollu:** skemmamigrašuvnnat ja dáhtoluođđasitgieđahallan leat dáhtolul váivves. Doallevaš veardádallan.

### 2. OCR ja jorgaleapmi doibmet báikkálaš LLM:s, eai ge balva-API:s

OCR: open-source-mohtor, Tesseract CLI várenuppožuhttin. Jorgaleapmi + NER: Gemma Ollama bokte, Apple Silicon laptopas.

**Manin:** nolla margina-gollu juohke áššegirjji ovddas; ođđasitbuvttadanlágan (fásta modealla + gohččumat); ja viežžandássi ferte juo doaimmadit ruokto-IP:s (gáldu lea Akamai Bot Manager duohken — `curl` oažžu 403), nu ahte laptop lea juo mielde.

**Gollu:** jorgalankvalitehta lea vuolilt go ovdamodealla. Referánssakorpusas gos originála eaŋgalasgielalaš veršuvdna lea álo ovtta deaddileami duohken, dat lea buorre. Mii eat čuoččo ahte jorgalusat leat virggálaččat.

### 3. Goappaš beali juogadit juste ovtta gaskabottasa: almmuhuvvon buđaldusa

Pipeline ii goassege čále njuolga buokčindieđuidvuorkái. Dat buktá `{ SQL, aktiva-manifessta, gaskaboddasiid-buhttenlisttu }`. "Almmuheapmi" = geavat dan buđaldusa ovddosguvlui (deatte SQL edge-SQL-DB:i, synkronisere aktivaid objeavttavuorkái, buhtte namuhuvvon gaskaboddasiid-čoavdagiid).

**Manin:** báikkálaš bealli ja edge-bealli sáhttiba ovdánit iehčanasat; buđaldus lea dárkunlágan; ja "almmuhit dieđuid" lea seamma hápmi juohke háve. Worker lea unna TypeScript/Hono-prográmma — čavga CSP (ii `unsafe-inline`; siskkildahttojuvvon JSON-LD lea sha256-nanistuvvon), `Accept-Language` + riika→giella-negošieren, 30-beaivvi KV-siidogaskaboddas, beaivválaš dállodoallo-cron — iige dat goassege dárbbaš diehtit mo dáhta lea ráhkaduvvon.

**Gollu:** D1 skemmarievdadus guoská guovtti fiilai (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Hálbmes sihkkarvuohta.

### Ii-negošerenlágan iešvuođat mat leat oassin láhttenis

- Ii leat čadnon U.S. ráđđehussii; eai virggálaš dovdomearkkat.
- Gáldosensureringat seailluhuvvojit, eai goassege guoroluvvo.
- Video čujuhuvvon DVIDS / AARO:i.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` siidoviidosaččat — ohcanindekserenlágan, AI-viežžamis eret válljen.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
