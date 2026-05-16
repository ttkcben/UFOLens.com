# GitHub – Příspěvek 1 ze 3 · Oznámení o vydání / blok pro README

**Použití jako:** tělo GitHub Release, připnutá diskuze nebo horní část souboru README repozitáře.
**Klíčová slova:** UAP, UFO, archiv PURSUE, odtajněné dokumenty, otevřená data, fulltextové vyhledávání, OCR, strojový překlad, lokální LLM, Ollama, edge computing, veřejné API, Hono, TypeScript, Python
**Hypertextové odkazy:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com – vícejazyčná, prohledávatelná platforma pro archiv PURSUE UAP

**Živě:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Zdrojový archiv:** https://www.war.gov/ufo

`ufolens.com` znovu publikuje archiv **PURSUE** odtajněných záznamů o UAP / UFO amerického Ministerstva války jako znalostní platformu: fulltextové vyhledávání, strojový překlad napříč korpusem, prozkoumávání mapy a časové osy a veřejné JSON API. Zdrojové dokumenty jsou dílem federální vlády USA a v rámci USA jsou public domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Tento projekt **není spojen s vládou USA**, nepoužívá žádné oficiální insignie a nikdy neodstraňuje redakční úpravy.

### Architektura

```
Lokální stroj (Apple Silicon, rezidenční IP)         Edge síť
──────────────────────────────────────────          ──────────────────────────
pipeline/ (Python 3.10, jádro pouze stdlib)          worker/ (TypeScript, Hono.js)
  fetch → OCR → translate → publish (pouze vpřed)      /{lang}/...   stránky
  OCR: open-source engine (Tesseract CLI jako záloha)  /api/v1/...   veřejné API
  translate / NER: lokální LLM (Gemma přes Ollama)     /admin        konzole operátora
  stav: manifest v SQLite                            podporováno: edge SQL DB, object
        │                                              storage (zdrojové PDF), KV cache
        └── publikuje balíček: SQL + manifest aktiv + seznam pro vyčištění cache ──┘
```

- **Nulové náklady na cloudovou AI na jeden dokument.** OCR a překlad běží lokálně; stavový automat pouze pro posun vpřed (`objeveno → staženo → ocr_hotovo → přeloženo → publikováno`) zaručuje, že žádný dokument není znovu zpracován, pokud se nezměnil.
- **Jádro pipeline nemá žádné závislosti na třetích stranách** – moduly pro parsování / manifest / deltu běží a testují se na čistém Pythonu bez jakýchkoli instalací přes pip; fáze OCR/překladu se elegantně degradují, pokud chybí volitelné balíčky.
- **Edge web** aplikuje striktní bezpečnostní hlavičky + CSP (žádné `unsafe-inline`; inline JSON-LD je připnutý pomocí sha256), vyjednávání jazyka přes `Accept-Language` + mapování zemí, 30denní KV cache stránek a denní údržbový cron.
- **Inkrementální aktualizace:** detektor delty porovnává zdrojový index a předává do pipeline pouze změny.

### Pro vývojáře

Veřejné API na https://www.ufolens.com/api/v1 vrací dokumenty a metadata jako JSON. Anonymní přístup je omezen počtem požadavků; pro výzkumnické/vývojářské úrovně si vyžádejte klíč. Koncové body a limity najdete v sekci API na webu.

### Stav

Kód je kompletní; web je nasazen na https://www.ufolens.com. Produkční databáze se plní spuštěním offline pipeline a následným publikováním balíčku (`cli_publish run --remote`). Kompletní návrhová dokumentace se nachází v `docs/20260511/`.

### Licence / hranice

- Zdrojové dokumenty: díla federální vlády USA, public domain v rámci USA.
- Vlastní kód této platformy: viz `LICENSE`.
- Web odesílá `Tdm-Reservation: 1` a `X-Robots-Tag: noai, noimageai` – indexovatelný vyhledávači, odhlášený z trénování/scrapování AI.
- Videozáznamy jsou připisovány DVIDS / AARO a tento projekt si na ně nečiní nárok.

Problémy a PR jsou vítány. Před otevřením strukturálních změn si prosím přečtěte `CLAUDE.md` a `docs/20260511/00-*`.

