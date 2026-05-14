# GitHub — Khasiho 2 ho 3 · Pitso ea Batšehetsi / "litaba tse ntle tsa pele"

**Sebelisa e le:** Puisano e khomaretsoeng ("Contributing & good first issues") kapa kenyelletso ea CONTRIBUTING.md.
**Mantsoe a bohlokoa:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Lihokelo:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ho Tšehetsa ufolens.com

[ufolens.com](https://www.ufolens.com) e fetola polokelo ea Lefapha la Ntoa la U.S. ea [PURSUE UAP](https://www.war.gov/ufo) hore e be sethala se ka batlisisoang, sa lipuo tse ngata se nang le [API ea sechaba](https://www.ufolens.com/api/v1). Ke likarolo tse peli — pipeline ea ho kenya data ea Python ea lehae (`pipeline/`) le app ea TypeScript/Hono edge (`worker/`) — li kopana sebakeng se le seng: bundle ea SQL + lisebelisoa tse phatlalalitsoeng.

Ha o hloke lintlha tsa bolokolohi tsa cloud ho tšehetsa. Li-module tsa mantlha tsa pipeline li sebelisa stdlib-only 'me liteko tsa Worker li sebetsa khahlanong le polokelo ea in-memory.

### Kemiso

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Moo thuso e molemo ka ho fetisisa

**i18n / localization** — `worker/src/i18n/ui-strings.json` ke mohloli oa li-string tsa UI. Tlhahlobo ea sebui sa lehae sa puo efe kapa efe e seng ea Senyesemane ke ea bohlokoa haholo: ho fumana liphoso tsa mochini tse sa nepahalang, ho lokisa mathata a RTL/layout, ho ntlafatsa maemo a lipuisano tsa puo.

**Boleng ba OCR** — ho sebetsana hantle le li-scan tsa khale tsa li-typewritten pele ho OCR; tlhahlobo e bapisang enjine ea open-source le Tesseract fallback ho maqephe a mohlala.

**Accessibility** — ho lekola maqephe a entsoeng (`worker/src/render/`) khahlanong le WCAG; CSP e matla (ha ho `unsafe-inline`), kahoo tharollo e tlameha ho sebetsa ka har'a seo.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, tlhaloso ea OpenAPI, mohlala oa bareki.

**Pipeline robustness** — litsela tse ling tsa ho theoha hantle, tlaleho e ntle ea tsoelo-pele, maemo a thata a delta-detection (`pipeline/lib/delta.py`).

**Litokomane** — `docs/20260511/` (繁體中文; `00-*` ke index). Liphetolelo tsa litokomane tsa moralo ho Senyesemane lia amoheleha.

### Melao ea mantlha

- Litsela tsohle li amana — morero o tlameha ho khoneha ho pholletsa le mechine. Ha ho litsela tse thata tse ngotsoeng.
- Se ke oa eketsa pip dependency ho pipeline *core* module. Mekhahlelo ea boikhethelo e ka sebelisa liphutheloana tsa boikhethelo, 'me e tlameha ho theoha hantle ntle le tsona.
- Se ke oa fokotsa mochini oa boemo o tsoelang pele feela — ke boleng ba litšenyehelo.
- Se ke oa kenya lets'oao la molao la 'muso oa U.S., 'me u se ke oa eketsa letho le fetolang li-redaction tsa mohloli.
- Liphetoho tsa D1 schema li ama lifaele **tse peli**: `pipeline/lib/manifest_schema.sql` le `db/schema.sql`.
- Liteko ka khoutu e ncha. Melaetsa ea Conventional-commit.

Bala `CLAUDE.md` le `docs/20260511/00-*` pele, ebe u bula bothata ho buisana ka letho la moralo pele ho PR.

