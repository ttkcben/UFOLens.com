# GitHub — 1/3 bejegyzés · Kiadás / README bejelentő blokk

**Felhasználás:** GitHub Release törzsszövegeként, rögzített Discussion-ként, vagy a repo README tetején.
**Kulcsszavak:** UAP, UFO, PURSUE archívum, titkosítás alól feloldott dokumentumok, nyílt adatok, teljes szöveges keresés, OCR, gépi fordítás, helyi LLM, Ollama, edge computing, nyilvános API, Hono, TypeScript, Python
**Hiperhivatkozások:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — egy többnyelvű, kereshető platform a PURSUE UAP archívumhoz

**Élesben:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Forrásarchívum:** https://www.war.gov/ufo

Az `ufolens.com` újra közzéteszi az Amerikai Egyesült Államok Hadügyminisztériumának **PURSUE** archívumát, amely titkosítás alól feloldott UAP / UFO feljegyzéseket tartalmaz, egy tudásplatform formájában: teljes szöveges kereséssel, gépi fordítással a teljes korpuszon, térképes + idővonali felfedezéssel és egy nyilvános JSON API-val. A forrásdokumentumok az USA szövetségi kormányának művei, és az USA-n belül közkincsnek minősülnek ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ez a projekt **nem áll kapcsolatban az USA kormányával**, nem használ hivatalos jelvényeket, és soha nem fordítja vissza a kitakarásokat.

### Architektúra

```
Helyi gép (Apple Silicon, lakossági IP)        Edge hálózat
─────────────────────────────────────────      ─────────────────────────
pipeline/  (Python 3.10, stdlib-only mag)       worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish (csak előre)  /{lang}/...   oldalak
  OCR: nyílt forráskódú motor (Tesseract CLI vészmegoldás) /api/v1/...   nyilvános API
  translate / NER: helyi LLM (Gemma Ollama-n keresztül)   /admin        operátori konzol
  állapot: SQLite manifest                       támogatja: edge SQL DB, objektum-
        │                                          tároló (forrás PDF-ek), KV cache
        └── közzétesz egy csomagot: SQL + asset manifest + cache-törlési lista ──┘
```

- **Nulla dokumentumonkénti felhő-AI költség.** Az OCR és a fordítás helyben fut; a csak előre haladó állapotgép (`discovered → downloaded → ocr_done → translated → published`) garantálja, hogy egy dokumentum sem kerül újra feldolgozásra, hacsak nem változott.
- **A pipeline magjának nincsenek külső függőségei** — az elemző / manifest / delta modulok tiszta Python környezetben futnak és tesztelhetők, pip-pel telepített csomagok nélkül; az OCR/fordítási szakaszok kecsesen degradálódnak, ha az opcionális csomagok hiányoznak.
- **Az edge webhely** szigorú biztonsági fejléceket + CSP-t alkalmaz (nincs `unsafe-inline`; az inline JSON-LD sha256-tal rögzített), nyelvi egyeztetést végez az `Accept-Language` + ország leképezés alapján, 30 napos KV oldal-gyorsítótárat használ, és napi karbantartó cron-t futtat.
- **Inkrementális frissítések:** egy delta detektor összehasonlítja a forrásindexet, és csak a változásokat táplálja vissza a pipeline-ba.

### Fejlesztőknek

A https://www.ufolens.com/api/v1 címen elérhető nyilvános API dokumentumokat és metaadatokat ad vissza JSON formátumban. Az anonim hozzáférés korlátozott; kérjen kulcsot kutatói/fejlesztői szintekhez. Tekintse meg az API szekciót a webhelyen a végpontokért és a limitekért.

### Állapot

A kód kész; a webhely a https://www.ufolens.com címen telepítve van. A termelési adatbázis az offline pipeline futtatásával és a csomag előre történő közzétételével (`cli_publish run --remote`) van feltöltve. A teljes tervezési dokumentáció a `docs/20260511/` mappában található.

### Licenc / határok

- Forrásdokumentumok: Az USA szövetségi kormányának művei, az USA-n belül közkincs.
- Ennek a platformnak a saját kódja: lásd a `LICENSE` fájlt.
- A webhely `Tdm-Reservation: 1` és `X-Robots-Tag: noai, noimageai` fejléceket küld — a keresőmotorok indexelhetik, de az AI képzésből/adatgyűjtésből ki van zárva.
- A videófelvételek a DVIDS / AARO tulajdonát képezik, és ez a projekt nem tart rájuk igényt.

Issue-kat és PR-okat szívesen fogadunk. Kérjük, olvassa el a `CLAUDE.md` és a `docs/20260511/00-*` dokumentumokat, mielőtt strukturális változtatásokat nyitna.

