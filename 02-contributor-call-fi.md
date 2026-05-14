# GitHub – Julkaisu 2/3 · Osallistumiskutsu / "hyvät ensimmäiset issuet"

**Käyttö:** Kiinnitettynä keskusteluna ("Osallistuminen & hyvät ensimmäiset issuet") tai CONTRIBUTING.md-johdantona.
**Avainsanat:** avoin lähdekoodi, osallistuminen, hyvä ensimmäinen issue, i18n, lokalisointi, OCR, Python, TypeScript, Vitest, pytest, saavutettavuus, UAP, avoin data
**Hyperlinkit:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Osallistuminen ufolens.com-projektiin

[ufolens.com](https://www.ufolens.com) muuttaa Yhdysvaltain sotaministeriön [PURSUE UAP -arkiston](https://www.war.gov/ufo) haettavaksi, monikieliseksi alustaksi, jolla on [julkinen API](https://www.ufolens.com/api/v1). Se koostuu kahdesta osasta – paikallisesta Python-syöttöputkesta (`pipeline/`) ja TypeScript/Hono-reunasovelluksesta (`worker/`) – jotka kohtaavat yhdessä rajapinnassa: julkaistussa SQL + resurssipaketissa.

Et tarvitse pilvitunnuksia osallistuaksesi. Putken ydinmoduulit ovat vain stdlib-pohjaisia ja Worker-testit ajetaan muistinsisäistä tallennusta vasten.

### Asennus

```bash
# putki
python3 -m pytest pipeline/tests/          # kaikkien tulisi olla vihreitä, ei vaadi pip-asennusta

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Missä apu on hyödyllisintä

**i18n / lokalisointi** — `worker/src/i18n/ui-strings.json` on käyttöliittymän merkkijonojen lähde. Äidinkielisen puhujan tarkistus mille tahansa ei-englanninkieliselle lokaalille on erittäin arvokasta: korjaa kömpelöitä konekäännöksiä, korjaa RTL/asetteluongelmia, paranna kielineuvottelun reuna-tapauksia.

**OCR-laatu** — parempi esikäsittely vanhoille kirjoituskoneella kirjoitetuille skannauksille ennen OCR:ää; arviointikehikko, joka vertaa avoimen lähdekoodin moottoria Tesseract-varavaihtoehtoon näytesivuilla.

**Saavutettavuus** — auditioi renderöidyt sivut (`worker/src/render/`) WCAG:tä vasten; CSP on tiukka (ei `unsafe-inline`), joten ratkaisujen on toimittava sen puitteissa.

**API-ergonomia** — `worker/src/routes/` — sivutus, suodatus, OpenAPI-kuvaus, esimerkkiklientit.

**Putken vankkuus** — enemmän sulavia vikasietopolkuja, parempaa edistymisen raportointia, delta-tunnistuksen reuna-tapauksia (`pipeline/lib/delta.py`).

**Dokumentaatio** — `docs/20260511/` (繁體中文; `00-*` on hakemisto). Suunnitteluasiakirjojen käännökset englanniksi ovat tervetulleita.

### Perussäännöt

- Kaikki polut suhteellisia — projektin on oltava siirrettävissä koneiden välillä. Ei kovakoodattuja absoluuttisia polkuja.
- Älä lisää pip-riippuvuutta putken *ydin*moduuliin. Valinnaiset vaiheet voivat käyttää valinnaisia paketteja, ja niiden on toimittava rajoitetusti ilman niitä.
- Älä heikennä vain eteenpäin suuntautuvaa tilakonetta — se on kustannuskatto.
- Älä lisää virallisia Yhdysvaltain hallituksen tunnuksia, äläkä lisää mitään, mikä kumoaa lähdeasiakirjojen toimituksellisia poistoja.
- D1-skeemamuutokset koskettavat **kahta** tiedostoa: `pipeline/lib/manifest_schema.sql` ja `db/schema.sql`.
- Testit uuden koodin mukana. Conventional-commit-viestit.

Lue ensin `CLAUDE.md` ja `docs/20260511/00-*`, ja avaa sitten issue keskustellaksesi rakenteellisista asioista ennen PR:n tekemistä.
