# GitHub — Færsla 1 af 3 · Útgáfutilkynning / README kubbur

**Nota sem:** Innihald fyrir GitHub Release, festar umræður eða efst í README skrá.
**Lykilorð:** UAP, UFO, PURSUE archive, aflétt leynd af skjölum, opin gögn, fulltextaleit, OCR, vélþýðing, staðbundið LLM, Ollama, brúnnet (edge computing), opið API, Hono, TypeScript, Python
**Tenglar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — fjöltyngdur, leitanlegur vettvangur fyrir PURSUE UAP skjalasafnið

**Í loftinu:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Frumskjalasafn:** https://www.war.gov/ufo

`ufolens.com` endurútgefur **PURSUE** skjalasafn bandaríska varnarmálaráðuneytisins yfir afléttum UAP / UFO skjölum sem þekkingarvettvang: fulltextaleit, vélþýðingu yfir allt safnið, korta- og tímalínukönnun og opið JSON API. Frumskjölin eru verk bandarískra alríkisstjórnvalda og eru almenningseign innan Bandaríkjanna ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Þetta verkefni er **ekki tengt bandarískum stjórnvöldum**, notar engin opinber merki og afmáir aldrei yfirstrikanir.

### Arkitektúr

```
Staðbundin vél (Apple Silicon, heimilis-IP)       Brúnnet (Edge network)
──────────────────────────────────────────         ──────────────────────────
pipeline/  (Python 3.10, aðeins stdlib kjarni)     worker/  (TypeScript, Hono.js)
  sækja → OCR → þýða → gefa út  (aðeins áfram)       /{lang}/...   síður
  OCR: opinn hugbúnaður (Tesseract CLI til vara)    /api/v1/...   opið API
  þýðing / NER: staðbundið LLM (Gemma í gegnum Ollama) /admin        stjórnborð
  staða: SQLite manifest                           stutt af: edge SQL DB, hlutageymsla
        │                                            (frum-PDF), KV skyndiminni
        └── gefur út pakka: SQL + eignamanifest + skyndiminni-hreinsunarlisti ──┘
```

- **Enginn kostnaður á skjal vegna gervigreindar í skýinu.** OCR og þýðing keyra staðbundið; áfram-aðeins stöðuvélin (`discovered → downloaded → ocr_done → translated → published`) tryggir að ekkert skjal sé endurunnið nema það hafi breyst.
- **Kjarni pípunnar hefur engar þriðja aðila ósjálfstæði** — þáttunar- / manifest- / delta-einingar keyra og prófast á hreinum Python án nokkurs pip-uppsetts; OCR/þýðingarstigin virka með minni afköstum þegar valkvæðir pakkar eru fjarverandi.
- **Brúnvefurinn (Edge site)** notar strangar öryggisfyrirsagnir + CSP (ekkert `unsafe-inline`; innbyggt JSON-LD er sha256-pinnað), tungumálastjórnun í gegnum `Accept-Language` + landakortlagningu, 30 daga KV skyndiminni fyrir síður og daglegan viðhalds-cron.
- **Stigvaxandi uppfærslur:** delta-skynjari ber saman frumskrá og sendir aðeins breytingar aftur inn í pípuna.

### Fyrir forritara

Opna API-ið á https://www.ufolens.com/api/v1 skilar skjölum og lýsigögnum sem JSON. Nafnlaus aðgangur er takmarkaður; biðjið um lykil fyrir rannsóknar-/forritaraflokka. Sjáið API-hlutann á vefsíðunni fyrir endapunkta og takmarkanir.

### Staða

Kóði er tilbúinn; vefurinn er kominn í loftið á https://www.ufolens.com. Framleiðslugagnagrunnurinn er fylltur með því að keyra ótengdu pípuna og gefa út pakkann framvirkt (`cli_publish run --remote`). Öll hönnunarskjöl eru í `docs/20260511/`.

### Leyfi / takmarkanir

- Frumskjöl: Verk bandarískra alríkisstjórnvalda, almenningseign innan Bandaríkjanna.
- Kóði þessa vettvangs: sjá `LICENSE`.
- Vefurinn sendir `Tdm-Reservation: 1` og `X-Robots-Tag: noai, noimageai` — hann er verðtrygganlegur af leitarvélum, en hefur afþakkað notkun í þjálfun/sköfun gervigreindar.
- Myndbandsupptökur eru eignaðar DVIDS / AARO og eru ekki eign þessa verkefnis.

Ábendingar og PRs vel þegin. Vinsamlegast lesið `CLAUDE.md` og `docs/20260511/00-*` áður en lagðar eru til grundvallarbreytingar.
