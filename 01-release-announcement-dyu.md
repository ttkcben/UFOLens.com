# GitHub — Jɛɛ 1/3 · Bɔli / README kunnafoni
**Kɛcogo:** GitHub Release kunnafonisɛbɛn, Kulekan sinsinnen, walima depo README sanfɛla.
**KUMA KƐRƐNKƐRƐNNENW:** UAP, UFO, PURSUE sɛbɛnmarayɔrɔ, sɛbɛn minnu bɔra gundo la, data bɛɛ la, sɛbɛn kɔnɔ ɲinini, OCR, ɛntɛrinɛti ladamuni, LLM sigiyɔrɔ, Ollama, ɛntɛrinɛti lajɛ, API bɛɛ la, Hono, TypeScript, Python
**Jɛgɛrɛw:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP sɛbɛnmarayɔrɔ kan caman na, ɲinini bɛ se ka kɛ min kɔnɔ

**A bɛ yen:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Sɛbɛnmarayɔrɔ fɔlɔ:** https://www.war.gov/ufo

`ufolens.com` bɛ Ameriki ka Kɛlɛ Minisiriso ka **PURSUE** UAP / UFO sɛbɛn minnu bɔra gundo la, olu bɔra kokura i n’a fɔ dɔnniya platfɔmu: sɛbɛn kɔnɔ ɲinini bɛɛ, ɛntɛrinɛti ladamuni sɛbɛnw bɛɛ la, kariti + waati-jan-sen ɲinini, ani JSON API bɛɛ la. Sɛbɛn fɔlɔw ye Ameriki fanga ka baara ye, wa Ameriki kɔnɔ, u ye foroba ko ye ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Nin porojɛ in **tɛ jɛ ni Ameriki fanga ye**, a tɛ fanga ka taamasiyɛn si kɛ, a tɛ sɛbɛn minnu majigilenw yiracogo kɔsegi abada.

### Jɔcogo

```
Masini sigiyɔrɔ (Apple Silicon, so ka IP)     Ɛntɛrinɛti lajɛ
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only fɔlɔ)          worker/  (TypeScript, Hono.js)
  sara → OCR → ladamuni → bɔli (kɔfɛ-filɛli)      /{lang}/...   pahinw
  OCR: open-source engine (Tesseract CLI kɔfɛ-filɛli) /api/v1/...   API bɛɛ la
  ladamuni / NER: LLM sigiyɔrɔ (Gemma via Ollama)       /admin        baarakɛla ka konsolɛ
  hali: SQLite sɛbɛn                             a jɔlen bɛ: edge SQL DB, ko kɛrɛnkɛrɛnnenw
        │                                              marayɔrɔ (source PDFs), KV cache
        └── bɛ bɔli kɛ: SQL + nafolo sɛbɛn + cache-bɔli lisiti ──┘
```

- **Sɛbɛn kelen-kelen bɛɛ la, AI wari tɛ.** OCR ni ladamuni bɛ kɛ sigiyɔrɔ la; kɔfɛ-filɛli masini min bɛ kɔfɛ-filɛ (`a yecogo → a saracogo → ocr_kɛra → a ladamuninen → a bɔra`) b’a jira ko sɛbɛn si tɛ kɛ kokura fo ni a yɛlɛmana.
- **Pipeline fɔlɔ tɛ ni mɔgɔ sada ka jɛgɛrɛn si ye** — faranfasi / sɛbɛn / delta moduluw bɛ baara kɛ ka tɛsitɛri kɛ Python sanfɛ, n’o tɛ, pip si ma sigi; OCR/ladamuni waatiw bɛ jɔ ka ɲɛ ni pake opsyonɛliw tɛ yen.
- **Ɛntɛrinɛti lajɛ** bɛ sigida tɛgɛrɛw + CSP (a tɛ `unsafe-inline` kɛ; inline JSON-LD sha256-pinned), kan minɛcogo `Accept-Language` + jamana-kariti fɛ, tile 30 KV pahin cache, ani tile-bɛɛ ka so-saniya cron.
- **Yɛlɛma kura:** delta-yecogo-yɔrɔ bɛ sɛbɛn fɔlɔ faranfasi, wa a bɛ yɛlɛmaw dɔrɔn de kɔsegi pipeline kɔnɔ.

### Develɔpɛriw kama

API bɛɛ la, min bɛ https://www.ufolens.com/api/v1, o bɛ sɛbɛnw ni metadataw kɔsegi i n’a fɔ JSON. Mɔgɔ min tɛ dɔn, o ka don-ko bɛ datugun; klé ɲini ɲininkaliw/develɔpɛriw ka kuluw kama. API sigida lajɛ sitɛri kan ka kunkanw ni u danfɛnw ye.

### Hali

Kodi dafalen; sitɛri sigilen bɛ https://www.ufolens.com. Prodakson database bɛ sɔrɔ ni offline pipeline kɛli ye, ani ni bɔli kɛra kɔfɛ (`cli_publish run --remote`). Jɔcogo sɛbɛnw bɛɛ bɛ `docs/20260511/` kɔnɔ.

### Lisansi / danfɛnw

- Sɛbɛn fɔlɔw: Ameriki fanga ka baaraw, foroba ko Ameriki kɔnɔ.
- Nin platfɔmu in ka kodi yɛrɛ: `LICENSE` lajɛ.
- Sitɛri bɛ `Tdm-Reservation: 1` ni `X-Robots-Tag: noai, noimageai` ci — ɲinini-masiniw bɛ se k’u sɔrɔ, u bɔra AI kalan/sara-ko la.
- Video filimuw ye DVIDS / AARO de ta ye, wa nin porojɛ in t’u ta ye.

Ɲininkaliw ni PRw bɛ jaabi. `CLAUDE.md` ni `docs/20260511/00-*` kalan sanni i ka jɔcogo yɛlɛma si da wuli.

