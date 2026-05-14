# GitHub — Post 2 minn 3 · Sejħa għal kontributuri / "kwistjonijiet tajbin għall-bidu"

**Użu bħala:** Diskussjoni ffissata ("Kontribuzzjoni & kwistjonijiet tajbin għall-bidu") jew introduzzjoni għal CONTRIBUTING.md.
**Kliem ewlieni:** open source, kontribuzzjoni, kwistjoni tajba għall-bidu, i18n, lokalizzazzjoni, OCR, Python, TypeScript, Vitest, pytest, aċċessibbiltà, UAP, dejta miftuħa
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kontribuzzjoni għal ufolens.com

[ufolens.com](https://www.ufolens.com) jikkonverti l-arkivju [PURSUE UAP](https://www.war.gov/ufo) tad-Dipartiment tal-Gwerra tal-Istati Uniti fi pjattaforma li tista' titfittex u multilingwi b'[API pubblika](https://www.ufolens.com/api/v1). Hija magħmula minn żewġ nofsijiet — pipeline ta' inġestjoni lokali Python (`pipeline/`) u app edge TypeScript/Hono (`worker/`) — li jiltaqgħu f'interface waħda: pakkett ippubblikat ta' SQL + assi.

M'għandekx bżonn kredenzjali tal-cloud biex tikkontribwixxi. Il-moduli ewlenin tal-pipeline huma stdlib-only u t-testijiet tal-Worker jaħdmu kontra ħażna fil-memorja.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # għandu jkun kollu aħdar, l-ebda installazzjoni pip meħtieġa

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Fejn l-għajnuna hija l-aktar utli

**i18n / lokalizzazzjoni** — `worker/src/i18n/ui-strings.json` huwa s-sors tal-istringi tal-UI. Reviżjoni minn kelliem nattiv ta' kwalunkwe lokalità mhux Ingliża hija ta' valur kbir: aqbad output tal-magna li ma jinstemax tajjeb, irranġa problemi ta' RTL/tqassim, tejjeb każijiet estremi ta' negozjar tal-lingwa.

**Kwalità tal-OCR** — ipproċessar minn qabel aħjar ta' skansijiet qodma ttajpjati qabel l-OCR; arneż ta' evalwazzjoni li jqabbel il-magna open-source mal-fallback Tesseract fuq paġni kampjun.

**Aċċessibbiltà** — ivverifika l-paġni rrenduti (`worker/src/render/`) kontra l-WCAG; is-CSP huwa strett (l-ebda `unsafe-inline`), għalhekk is-soluzzjonijiet iridu jaħdmu f'dak il-qafas.

**Ergonomija tal-API** — `worker/src/routes/` — paġinazzjoni, filtrazzjoni, deskrizzjoni OpenAPI, klijenti eżempju.

**Robustezza tal-Pipeline** — aktar mogħdijiet ta' degradazzjoni grazzjuża, rappurtar tal-progress aħjar, każijiet estremi ta' ditezzjoni tad-delta (`pipeline/lib/delta.py`).

**Dokumenti** — `docs/20260511/` (繁體中文; `00-*` huwa l-indiċi). Traduzzjonijiet tad-dokumenti tad-disinn għall-Ingliż huma milqugħa.

### Regoli bażiċi

- Il-mogħdijiet kollha huma relattivi — il-proġett irid ikun portabbli bejn il-magni. L-ebda mogħdija assoluta kodifikata.
- Tżidx dipendenza pip ma' modulu *ewlieni* tal-pipeline. Stadji mhux obbligatorji jistgħu jużaw pakketti mhux obbligatorji, u għandhom jiddegradaw b'mod grazzjuż mingħajrhom.
- Tdgħajjifx il-magna tal-istat li timxi 'l quddiem biss — dak huwa l-limitu massimu tal-ispiża.
- Tżidx emblemi uffiċjali tal-gvern tal-Istati Uniti, u tżidx xejn li jreġġa' lura r-redazzjonijiet tas-sors.
- Il-bidliet fl-iskema D1 imissu **żewġ** fajls: `pipeline/lib/manifest_schema.sql` u `db/schema.sql`.
- Testijiet b'kodiċi ġdid. Messaġġi ta' commit konvenzjonali.

Aqra `CLAUDE.md` u `docs/20260511/00-*` l-ewwel, imbagħad iftaħ kwistjoni biex tiddiskuti xi ħaġa strutturali qabel il-PR.

