# GitHub — 3/3 bejegyzés · Architektúrális jegyzetek (ADR-stílusú Discussion)

**Felhasználás:** Discussion-ként a "Show and tell" / "Architecture" alatt, vagy `docs/` ADR kiindulópontként.
**Kulcsszavak:** architektúra, ADR, csak előre haladó állapotgép, helyi LLM, Ollama, OCR, edge computing, CSP, biztonsági fejlécek, adat-pipeline, költségtervezés, SQLite manifest, D1, R2, KV
**Hiperhivatkozások:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Miért épül fel az ufolens.com úgy, ahogy

Jegyzetek arról a három döntésről, amelyek formálták az [ufolens.com](https://www.ufolens.com) oldalt (a [PURSUE UAP archívum](https://www.war.gov/ufo) kereshető, többnyelvű újraépítését). A megjegyzéseket / kritikát szívesen fogadjuk.

### 1. A pipeline szándékosan egy csak előre haladó állapotgép

Állapotok: `discovered → downloaded → ocr_done → translated → published`. Egy dokumentum csak előre mozog, és csak akkor, ha van vele munka. A közzétett tartalom soha nem kerül újra feldolgozásra, hacsak egy delta detektor nem észleli, hogy a forrás ténylegesen megváltozott.

**Miért:** Az OCR + fordítás a költséges műveletek, és az archívum idővel növekszik. Egy olyan pipeline, amely "biztonság kedvéért mindent újra lefuttat", korlátlan költségekkel jár. A visszalépések lehetetlenné tétele lehetetlenné teszi az elszabadult költségeket. A költségplafon az állapotgráf tulajdonsága, nem pedig az operátor éberségéé.

**Ára:** a séma migrációk és a szándékos újrafeldolgozás szándékosan nehézkesek. Elfogadható kompromisszum.

### 2. Az OCR és a fordítás helyi LLM-en fut, nem felhő API-n

OCR: nyílt forráskódú motor, Tesseract CLI vészmegoldás. Fordítás + NER: Gemma Ollama-n keresztül, egy Apple Silicon laptopon.

**Miért:** nulla marginális költség dokumentumonként; reprodukálható (rögzített modell + promptok); és a letöltési lépésnek már amúgy is lakossági IP-címről kell futnia (a forrás Akamai Bot Manager mögött van — a `curl` 403-at kap), tehát egy laptop mindenképp a folyamat része.

**Ára:** a fordítás minősége elmarad egy csúcsmodelltől. Egy referencia korpusz esetében, ahol az eredeti angol mindig egy kattintásnyira van, ez rendben van. Nem állítjuk, hogy a fordítások hitelesek.

### 3. A két fél pontosan egy interfészen osztozik: egy közzétett csomagon

A pipeline soha nem ír közvetlenül a termelési adatbázisba. Kibocsát egy `{ SQL, asset manifest, cache-törlési lista }` csomagot. A "közzététel" = a csomag előre történő alkalmazása (SQL feltöltése az edge SQL DB-be, eszközök szinkronizálása az objektumtárolóba, a megnevezett gyorsítótár-kulcsok törlése).

**Miért:** a helyi és az edge oldal egymástól függetlenül fejlődhet; a csomag felülvizsgálható; és az "adatok telepítése" minden alkalommal ugyanolyan formájú. A Worker egy kicsi TypeScript/Hono alkalmazás — szigorú CSP (nincs `unsafe-inline`; az inline JSON-LD sha256-tal rögzített), `Accept-Language` + ország→nyelv egyeztetés, 30 napos KV oldal-gyorsítótár, napi karbantartó cron —, és soha nem kell tudnia, hogyan készültek az adatok.

**Ára:** egy D1 séma módosítása két fájlt érint (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Olcsó biztosítás.

### A viselkedésbe égetett, nem vitatható alapelvek

- Nem áll kapcsolatban az USA kormányával; nincsenek hivatalos jelvények.
- A forrás kitakarásai megmaradnak, soha nem kerülnek visszafordításra.
- A videók a DVIDS / AARO-nak tulajdonítva.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` az egész oldalon — kereső által indexelhető, AI adatgyűjtésből kizárva.

Élesben: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

