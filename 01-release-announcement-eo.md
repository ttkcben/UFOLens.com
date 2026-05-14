# GitHub — Afiŝo 1 el 3 · Anonco pri Lanĉo / README

**Uzu kiel:** korpo de GitHub-eldono, alpinglita diskuto, aŭ la supro de la README de la reponejo.
**Ŝlosilvortoj:** UAP, UFO, arkivo PURSUE, malsekretigitaj dokumentoj, malfermaj datumoj, plenteksta serĉo, OCR, maŝintradukado, loka LLM, Ollama, randa komputado, publika API, Hono, TypeScript, Python
**Hiperligoj:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — plurlingva, serĉebla platformo por la arkivo PURSUE pri UAP

**Retejo:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Fonta arkivo:** https://www.war.gov/ufo

`ufolens.com` re-publikigas la arkivon **PURSUE** de la Usona Departemento de Milito pri malsekretigitaj registroj de UAP / UFO kiel scio-platformo: plenteksta serĉo, maŝintradukado tra la tuta korpuso, esplorado per mapo + templinio, kaj publika JSON API. La fontaj dokumentoj estas verkoj de la usona federacia registaro kaj, ene de Usono, estas en la publika domajno ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ĉi tiu projekto **ne estas ligita al la usona registaro**, uzas neniujn oficialajn insignojn, kaj neniam malfaras redaktojn.

### Arkitekturo

```
Loka maŝino (Apple Silicon, hejma IP)              Randa reto
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, kerno nur kun stdlib)     worker/  (TypeScript, Hono.js)
  kapti → OCR → traduki → publikigi (nur-antaŭen)      /{lang}/...   paĝoj
  OCR: malfermfonta motoro (Tesseract CLI rezervo)     /api/v1/...   publika API
  traduki / NER: loka LLM (Gemma per Ollama)           /admin        operacianta konzolo
  stato: SQLite manifesto                            subtenata de: randa SQL DB, objekta
        │                                              stokado (fontaj PDF-oj), KV kaŝmemoro
        └── publikigas pakaĵon: SQL + aktivaĵa manifesto + listo por kaŝmemor-purigado ──┘
```

- **Nul kosto de nuba AI por ĉiu dokumento.** OCR kaj tradukado ruliĝas loke; la nur-antaŭen statmaŝino (`malkovrita → elŝutita → ocr_farita → tradukita → publikigita`) garantias, ke neniu dokumento estas reprocesita krom se ĝi ŝanĝiĝis.
- **La kerno de la dukto ne havas dependecojn de triaj partioj** — la moduloj por analizo / manifesto / diferencoj ruliĝas kaj testas sur pura Python sen iuj `pip`-instalitaj pakaĵoj; la OCR/tradukaj stadioj gracie degeneras kiam nedevigaj pakaĵoj mankas.
- **La randa retejo** aplikas striktajn sekurecajn kapliniojn + CSP (neniu `unsafe-inline`; enlinia JSON-LD estas fiksita per `sha256`), lingvan negocadon per `Accept-Language` + landa mapado, 30-tagan KV-paĝan kaŝmemoron, kaj ĉiutagan purigan kron-taskon.
- **Kreskaj ĝisdatigoj:** diferenc-detektilo komparas la fontan indekson kaj provizas nur ŝanĝojn reen en la dukton.

### Por programistoj

La publika API ĉe https://www.ufolens.com/api/v1 redonas dokumentojn kaj metadatumojn kiel JSON. Anonima aliro estas rapideco-limigita; petu ŝlosilon por esploristaj/programistaj niveloj. Vidu la API-sekcion en la retejo por finpunktoj kaj limoj.

### Stato

Kodo kompleta; retejo deplojita ĉe https://www.ufolens.com. La produkta datumbazo estas plenigita per rulado de la senreta dukto kaj publikigado de la pakaĵo antaŭen (`cli_publish run --remote`). Plenaj dezajnaj dokumentoj troviĝas en `docs/20260511/`.

### Permesilo / limoj

- Fontaj dokumentoj: verkoj de la usona federacia registaro, publika domajno ene de Usono.
- La propra kodo de ĉi tiu platformo: vidu `LICENSE`.
- La retejo sendas `Tdm-Reservation: 1` kaj `X-Robots-Tag: noai, noimageai` — indeksebla de serĉmotoroj, malaliĝis de trejnado/skrapado de AI.
- Videofilmoj estas atribuitaj al DVIDS / AARO kaj ne estas pretendataj de ĉi tiu projekto.

Problemoj kaj PR-oj estas bonvenaj. Bonvolu legi `CLAUDE.md` kaj `docs/20260511/00-*` antaŭ ol malfermi strukturajn ŝanĝojn.

