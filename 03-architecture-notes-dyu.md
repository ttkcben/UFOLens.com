# GitHub — Jɛɛ 3/3 · Jɔcogo kunnafonisɛbɛnw (ADR-style Discussion)

**A kɛcogo:** Kulekan "Yira ni Fɔ" / "Jɔcogo" kɔnɔ, walima `docs/` ADR sow.
**KUMA KƐRƐNKƐRƐNNENW:** jɔcogo, ADR, kɔfɛ-filɛli masini, LLM sigiyɔrɔ, Ollama, OCR, ɛntɛrinɛti lajɛ, CSP, sigida tɛgɛrɛw, data pipeline, wari jɔcogo, SQLite sɛbɛn, D1, R2, KV
**Jɛgɛrɛw:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mun na ufolens.com jɔra ten

Kunnafonisɛbɛn minnu bɛ [ufolens.com](https://www.ufolens.com) jɔcogo kan ( [PURSUE UAP sɛbɛnmarayɔrɔ](https://www.war.gov/ufo) kokura min bɛ ɲinini, kan caman na). Kuma/kɔlɔsiliw bɛ jaabi.

### 1. Pipeline ye kɔfɛ-filɛli masini ye min bɛ kɔfɛ-filɛ — n’a dalen b’a la

Haliw: `a yecogo → a saracogo → ocr_kɛra → a ladamuninen → a bɔra`. Sɛbɛn bɛ kɔfɛ-filɛ dɔrɔn, wa ni baara bɛ yen. Sɛbɛn min bɔra, o tɛ kɛ kokura abada fo ni delta-yecogo-yɔrɔ y’a ye ko sɛbɛn fɔlɔ yɛlɛmana yɛrɛ.

**Mun na:** OCR + ladamuni de ye baara warilenw ye, wa sɛbɛnmarayɔrɔ bɛ kuru waati bɛɛ. Pipeline min bɛ "ko bɛɛ kɛ kokura ka sigi", o wari tɛ dan ye. Ni kɔfɛ-filɛli kɛra ko man gɛlɛn, o bɛ wari-bili min bɛ bɔ, o kɛ ko man gɛlɛn ye. Wari danfɛ ye hali grafiki de ta ye, n’o tɛ baarakɛla ka lajɛli ye.

**Wari:** jɔcogo-yɛlɛma ni kokura-kɛli n’a dalen b’a la, olu gɛlɛya kosɛbɛ. Yɛlɛma min bɛ se ka sɔn.

### 2. OCR ni ladamuni bɛ kɛ LLM sigiyɔrɔ la, n’o tɛ cloud API ye

OCR: open-source engine, Tesseract CLI fallback. Ladamuni + NER: Gemma via Ollama, Apple Silicon laptop kan.

**Mun na:** sɛbɛn kelen-kelen bɛɛ la, wari si tɛ; a bɛ se ka kɛ kokura (modeli + ɲininiw sigilen); wa sara-cogo ka kan ka kɛ ka bɔ so ka IP la (bɔyɔrɔ bɛ Akamai Bot Manager kɔ — `curl` bɛ 403 sɔrɔ), o la, laptop bɛ baara la.

**Wari:** ladamuni kalite bɛ danfɛ modeli kɔ. Sɛbɛnmarayɔrɔ kama, min kɔnɔ Angilɛkan fɔlɔ bɛ kiliki kelen dɔrɔn de la, o ɲɛ. An t’a fɔ ko ladamuniw ye fanga ta ye.

### 3. Fɛn fila bɛɛ bɛ yɔrɔ kelen dɔrɔn de la: bɔli min bɔra

Pipeline tɛ sɛbɛn kɛ prodakson database la abada. A bɛ `{ SQL, nafolo sɛbɛn, cache-bɔli lisiti }` bɔ. "Bɔli" = o bɔli kɛ kɔfɛ (SQL pusi edge SQL DB la, nafolow jɛ ka taa ko kɛrɛnkɛrɛnnenw marayɔrɔ la, cache klé minnu tɔgɔw dalen, olu bɔ).

**Mun na:** sigiyɔrɔ ni ɛntɛrinɛti lajɛ bɛ se ka yiriwa yɛrɛmahɔrɔnya la; bɔli bɛ se ka lajɛ; wa "data deploy" ye cogo kelen ye waati bɛɛ. Worker ye TypeScript/Hono app fitinin ye — CSP ka gɛlɛn (a tɛ `unsafe-inline` kɛ; inline JSON-LD ye sha256-pinned ye), `Accept-Language` + jamana→kan minɛcogo, tile 30 KV pahin cache, tile-bɛɛ ka so-saniya cron — wa a tɛna a dɔn abada data kɛra cogo min na.

**Wari:** D1 jɔcogo yɛlɛma bɛ dosiye fila de sɔrɔ (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Siraɲuman wari-fin.

### Ko minnu tɛ se ka yɛlɛma, olu bɛ kɛcogo la

- A tɛ jɛ ni Ameriki fanga ye; fanga ka taamasiyɛn si tɛ.
- Sɛbɛn fɔlɔw majigilenw bɛ mara, u tɛ kɔsegi abada.
- Video ye DVIDS / AARO de ta ye.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` sitɛri bɛɛ la — ɲinini-sɛbɛn bɛ se ka kɛ, AI-sara-bɔra.

A bɛ yen: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
