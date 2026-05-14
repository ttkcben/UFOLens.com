# GitHub — Xiviko 3 hi 3 · Matsalwa ya Vukatsi (Nkangano wa ADR-style)

**Tirhisa tanihi:** Nkangano ehansi ka "Kombisa na ku byela" / "Vukatsi", kumbe `docs/` ADR seed.
**Marito-nkulu:** vukatsi, ADR, muchini wa xiyimo xo famba mahlweni ntsena, LLM ya kwala, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Hikokwalaho ka yini ufolens.com yi akiwile hi ndlela leyi

Matsalwa hi swiboho swinharhu leswi vumbeke [ufolens.com](https://www.ufolens.com) (ku aka nakambe ka tindzimi to tala, lexi kambisisekaka xa [PURSUE UAP archive](https://www.war.gov/ufo)). Marito / ku kaneta swa amukeleka.

### 1. Pipeline i muchini wa xiyimo xo famba mahlweni ntsena — hi xikongomelo

Swiyimo: `discovered → downloaded → ocr_done → translated → published`. Document yi famba mahlweni ntsena, naswona ntsena loko ku ri na ntirho wo endla. Switirho leswi huxiweke a swi tlheli swi tirhisiwa handle ka loko delta detector yi vona xihlovo xi cincile.

**Hikokwalaho ka yini:** OCR + vuhundzuluxi i mintirho ya mali leyi tlakukeke, naswona vuhlayiselo bya kula hi nkarhi. Pipeline leyi "tlhetelaka yi tirhisa hinkwaswo ku va yi sirhelelekile" yi na mali leyi nga na ndzilakano. Ku endla leswaku ku famba hi vukati ku nga koteki swi endla leswaku mali leyi humaka yi nga koteki. Ntengo wa le henhla i xipimana xa state graph, ku nga ri ku tivonelela ka mutirhisi.

**Mali:** schema migrations na reprocessing-on-purpose swi nonon'hwerile hi xikongomelo. Ku cincana loku amukelekaka.

### 2. OCR na vuhundzuluxi swi tirha eka LLM ya kwala, ku nga ri cloud API

OCR: open-source engine, Tesseract CLI fallback. Vuhundzuluxi + NER: Gemma hi Ollama, eka laptop ya Apple Silicon.

**Hikokwalaho ka yini:** zero mali yo engetela hi document; yi nga tlhetleriwa (fixed model + prompts); naswona fetch step yi fanele ku tirha hi residential IP (xihlovo xi le ximatsanini xa Akamai Bot Manager — `curl` yi kuma 403), hikokwalaho laptop yi le ka loop hambi swi ri tano.

**Mali:** vun'wana bya vuhundzuluxi byi le hansi ka frontier model. Eka reference corpus laha Xinghezi xa xitshuriwa xi nga nkarhi hinkwawo hi click yin'we, swi lulamile. A hi vuli leswaku vuhundzuluxi bya tiyisiwa.

### 3. Tihakelo timbirhi ti avelana interface yin'we ntsena: published bundle

Pipeline a yi tshuki yi tsala eka production database hi tlhelo. Yi humesa `{ SQL, asset manifest, cache-purge list }`. "Ku humesa" = tirhisa bundle wolowo mahlweni (rhumela SQL eka edge SQL DB, sync assets eka object storage, sula named cache keys).

**Hikokwalaho ka yini:** tlhelo ra kwala na tlhelo ra edge swi nga kula hi ku tiyimela; bundle ra kambisisiwa; naswona "deploy data" ri na ximfumo xo fana nkarhi hinkwawo. Worker i TypeScript/Hono app leyitsongo — strict CSP (ku hava `unsafe-inline`; inline JSON-LD i sha256-pinned), `Accept-Language` + country→language negotiation, 30-day KV page cache, daily housekeeping cron — naswona a ri tshuki ri fanele ku tiva leswaku data ri endliwe njhani.

**Mali:** D1 schema change yi khumba tinhlayo timbirhi (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Inshuransi yo olova.

### Swilo leswi nga hlawulekiki leswi nghenisiweke eka mahanyelo

- A swi hlanganisiwi na mfumo wa U.S.; ku hava swikombiso swa ximfumo.
- Swihundla swa xihlovo swi hlayisiwa, a swi tshuki swi tlherisiwa.
- Vhidiyo yi vuriwa ya DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` eka site hinkwayo — yi nga kumeka hi search-index, AI-scrape-opted-out.

Hanya: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
