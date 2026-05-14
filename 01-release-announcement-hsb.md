# GitHub — Přispawk 1 z 3 · Wozjewjenje wudaća / README-blok

**Wužij jako:** Těsto GitHub-wudaća, připjaty slěd diskusije abo spočatk repo-README.
**Klučowe hesła:** UAP, UFO, PURSUE-archiw, deklasifikowane dokumenty, wotewrjene daty, połnotekstowe pytanje, OCR, mašinowy přełožk, lokalny LLM, Ollama, edge computing, zjawny API, Hono, TypeScript, Python
**Wotkazy:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — wjacerěčna, přepytać dacaca so platforma za PURSUE UAP-archiw

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Žórłowy archiw:** https://www.war.gov/ufo

`ufolens.com` znowa wozjewja **PURSUE**-archiw wójnskeho departmenta Zjednoćenych statow Ameriki z deklasifikowanymi UAP / UFO-zapiskami jako wědźenska platforma: połnotekstowe pytanje, mašinowy přełožk přez cyły korpus, přepytowanje přez kartu a časowu lajstu a zjawny JSON-API. Žórłowe dokumenty su dźěła zwjazkoweho knježerstwa ZSA a su w ZSA zjawne ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Tutón projekt **njeje z knježerstwom ZSA zwjazany**, njewužiwa žane oficielne znamjenja a ženje njewobroća redakcije.

### Architektura

```
Lokalna mašina (Apple Silicon, bydlenski IP)        Kromowa syć
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, jadro jenož ze stdlib)       worker/  (TypeScript, Hono.js)
  wzać → OCR → přełožić → wozjewić  (jenož doprědka)   /{lang}/...   strony
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   zjawny API
  přełožić / NER: lokalny LLM (Gemma přez Ollama)       /admin        konsola operatora
  staw: SQLite-manifest                              podeprěty wot: edge SQL DB, objektowe
        │                                              składowanišćo (žórłowe PDF), KV-cache
        └── wozjewja zwjazk: SQL + manifest zasobow + lisćina za wuprózdnjenje pufraka ──┘
```

- **Nulowe kóšty za mašinowu inteligencu w mróčeli na dokument.** OCR a přełožk běžitej lokalnje; mašina z stawom, kotraž dźěła jenož doprědka (`namakany → sćehnjeny → ocr_sčinjeny → přełoženy → wozjewjeny`), zawěsća, zo so žadyn dokument znowa njepředźěłuje, chibazo je so změnił.
- **Jadro pipeline nima žane wotwisnosće wot třećich poskićowarjow** — parsowanske / manifestowe / delta-moduly běža a testuja so na čisćym Python bjez wšoho, štož je přez pip instalowane; OCR/přełoženske schodźenki degraduja z graciju, hdyž faluje opcionalne pakćiki.
- **Edge-sydło** nałožuje krute wěstotne hłowy + CSP (nic `unsafe-inline`; inline JSON-LD je z sha256 připjaty), rěčne jednanie přez `Accept-Language` + zwobraznjenje kraja, 30-dny KV-pufrak za strony a dnjowy cron za porjadowanje.
- **Inkrementalne aktualizacije:** delta-detektor přirunuje žórłowy indeks a pósrědkuje jenož změny wróćo do pipeline.

### Za wuwiwarjow

Zjawny API na https://www.ufolens.com/api/v1 wróći dokumenty a metadaty jako JSON. Anonymny přistup je limitowany; naprašujće za klučom za slědźerske/wuwiwarske schodźenki. Hlejće wotrězk API na sydle za kónčne dypki a limity.

### Staw

Kod je hotowy; sydło je na https://www.ufolens.com zasadźene. Produkcisku datowu banku napjelnja so přez wuwjedźenje offline-pipeline a wozjewjenje zwjazka doprědka (`cli_publish run --remote`). Połne designowe dokumenty su w `docs/20260511/`.

### Licenca / hranicy

- Žórłowe dokumenty: Dźěła zwjazkoweho knježerstwa ZSA, w ZSA zjawne.
- Swójski kod tuteje platformy: hlej `LICENSE`.
- Sydło pósćela `Tdm-Reservation: 1` a `X-Robots-Tag: noai, noimageai` — indeksujomny přez pytawki, wotzjewjeny za trenowanje/scraping přez AI.
- Widejofilmy so DVIDS / AARO připisaja a so wot tutoho projekta njepožadaja.

Wjelkostatki a PRs wutrobnje witane. Prošu čitajće `CLAUde.md` a `docs/20260511/00-*` prjedy hač wočiniće strukturelne změny.

