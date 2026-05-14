# GitHub – 1 iš 3 įrašų · Išleidimo / README pranešimų blokas

**Naudoti kaip:** GitHub išleidimo turinį, prisegtą diskusiją arba saugyklos README failo viršuje.
**Raktažodžiai:** UAP, UFO, PURSUE archyvas, išslaptinti dokumentai, atviri duomenys, viso teksto paieška, OCR, mašininis vertimas, vietinis LLM, Ollama, edge computing, vieša API, Hono, TypeScript, Python
**Hipersaitai:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com – daugiakalbė, paieškos galimybę turinti platforma, skirta PURSUE UAP archyvui

**Veikia:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Pirminis archyvas:** https://www.war.gov/ufo

`ufolens.com` iš naujo skelbia JAV Karo departamento **PURSUE** išslaptintų UAP / UFO įrašų archyvą kaip žinių platformą: viso teksto paieška, mašininis vertimas visame korpuse, tyrinėjimas žemėlapyje ir laiko juostoje bei vieša JSON API. Pirminiai dokumentai yra JAV federalinės vyriausybės darbai ir JAV teritorijoje yra viešo naudojimo ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Šis projektas **nėra susijęs su JAV vyriausybe**, nenaudoja oficialios simbolikos ir niekada neatšaukia redagavimų.

### Architektūra

```
Vietinis kompiuteris (Apple Silicon, gyvenamosios vietos IP)   Edge tinklas
─────────────────────────────────────────                     ─────────────────────────
pipeline/  (Python 3.10, tik stdlib branduolys)                 worker/  (TypeScript, Hono.js)
  gauti → OCR → versti → skelbti (tik pirmyn)                     /{lang}/...   puslapiai
  OCR: atvirojo kodo variklis (Tesseract CLI atsarginis variantas) /api/v1/...   vieša API
  vertimas / NER: vietinis LLM (Gemma per Ollama)                  /admin        operatoriaus konsolė
  būsena: SQLite manifestas                                     paremta: edge SQL DB, objektų
        │                                                         saugykla (pirminiai PDF), KV talpykla
        └── skelbia paketą: SQL + išteklių manifestas + talpyklos valymo sąrašas ──┘
```

- **Nulinės debesijos AI sąnaudos vienam dokumentui.** OCR ir vertimas vykdomi vietoje; „tik pirmyn“ būsenų mašina (`discovered → downloaded → ocr_done → translated → published`) užtikrina, kad joks dokumentas nebūtų perdirbamas, nebent jis pasikeitė.
- **Dutotiekio branduolys neturi jokių trečiųjų šalių priklausomybių** – analizavimo / manifesto / delta moduliai veikia и testuojami su švaria Python versija, be jokių pip instaliacijų; OCR/vertimo etapai sklandžiai veikia su apribojimais, kai trūksta pasirenkamų paketų.
- **Edge svetainė** taiko griežtas saugumo antraštes + CSP (jokio `unsafe-inline`; įterptas JSON-LD sha256-prisegtas), kalbos derinimą per `Accept-Language` + šalies atvaizdavimą, 30 dienų KV puslapių talpyklą ir kasdienį tvarkymo cron darbą.
- **Inkrementiniai atnaujinimai:** delta detektorius palygina pirminį indeksą ir į dutotiekį perduoda tik pakeitimus.

### Kūrėjams

Vieša API adresu https://www.ufolens.com/api/v1 grąžina dokumentus и metaduomenis JSON formatu. Anoniminė prieiga yra ribojama; paprašykite rakto tyrėjų/kūrėjų lygmenims. Apie galinius taškus ir apribojimus skaitykite svetainės API skiltyje.

### Būsena

Kodas užbaigtas; svetainė įdiegta adresu https://www.ufolens.com. Gamybinė duomenų bazė užpildoma vykdant neprisijungusį dutotiekį ir skelbiant paketą pirmyn (`cli_publish run --remote`). Visi projektavimo dokumentai yra `docs/20260511/`.

### Licencija / ribojimai

- Pirminiai dokumentai: JAV federalinės vyriausybės darbai, viešo naudojimo JAV teritorijoje.
- Šios platformos kodas: žr. `LICENSE`.
- Svetainė siunčia `Tdm-Reservation: 1` ir `X-Robots-Tag: noai, noimageai` – indeksuojama paieškos sistemų, bet atsisakyta AI mokymui/duomenų rinkimui.
- Vaizdo įrašai priskiriami DVIDS / AARO ir šis projektas į juos nepretenduoja.

Klaidų pranešimai ir PR yra laukiami. Prieš atidarydami struktūrinius pakeitimus, perskaitykite `CLAUDE.md` ir `docs/20260511/00-*`.

