# GitHub — 2. nu 3 paziņuojumim · Aicynōjums dalinīkim / "lobi pyrmī uzdevumi"

**Lītuot, kai:** pīspraustu sarunu tematu ("Pīspaidu snēgšona & lobi pyrmī uzdevumi") voi CONTRIBUTING.md īvadu.
**Atslāgvārdi:** atvārtō ōlūta koda projekts, pīspaidu snēgšona, lobas pyrmōs problēmas, i18n, lokalizācija, OCR, Python, TypeScript, Vitest, pytest, pīejameiba, UAP, atvērtī dati
**Hipersaites:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Pīspaidu snēgšona ufolens.com projektam

[ufolens.com](https://www.ufolens.com) pōrveidoj ASV Kara departamenta [PURSUE UAP arhivu](https://www.war.gov/ufo) par meklējamu, daudzvalōdeigu platformu ar [publisku API](https://www.ufolens.com/api/v1). Tys sastōv nu divom daļom — vītejō Python datu apstrādes cauruļvada (`pipeline/`) un TypeScript/Hono malu aplikācijas (`worker/`) —, kas sasavej vīnā saskarnē: publicētā SQL + līdzekļu paketē.

Lai snāgtu sovu pīspaidu, jums nav vajadzeigi nivīni muokūņa pīejas dati. Cauruļvada pamatmoduļi ir tikai stdlib, un Worker testi dorbojās ar atmiņā īglobuotu krōtuvi.

### Īstōteišona

```bash
# pipeline
python3 -m pytest pipeline/tests/          # vysam vajadzātu byut zaļam, nav vajadzeigs pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kur paleidzeiba ir vysvairōk noderīga

**i18n / lokalizācija** — `worker/src/i18n/ui-strings.json` ir UI tekstu ōlūts. Dzimtōs valōdas runōtōju pōrbaude jebkurai valōdai, kas nav angļu, ir ļoti vērtīga: atrast naērtus mašīntlulkōjumus, izlabōt RTL/izkōrtōjuma problēmas un uzlabōt valōdas saskaņōšonas īpatnejus gadījumus.

**OCR kvalitāte** — lobōka vecu mašīnrakstītu skenējumu pyrmsapstrōde pyrms OCR; vierteišonas sistēma, kas salīdzynoj atvārtō ōlūta dzynēju ar Tesseract rezervis uz parauglopom.

**Pīejameiba** — pōrbaudit atveidōtōs lopas (`worker/src/render/`) attīceibā pret WCAG; CSP ir stingrs (nav `unsafe-inline`), tōpēc rysinōjumim ir jōdorbojās šōs rōmōs.

**API ergonomika** — `worker/src/routes/` — lopu šķiršona, filrēšona, OpenAPI apraksts, klientu pīmāri.

**Cauruļvada iztureiba** — vairōk graciozas degradācijas ceļu, lobōka progresu atskaitīšona, delta atklōšonas īpatnejī gadījumi (`pipeline/lib/delta.py`).

**Dokumentācija** — `docs/20260511/` (繁體中文; `00-*` ir indekss). Dizaina dokumentu tulkōjumi angļu valōdā ir gaideiti.

### Pamatnūsacejumi

- Vysim ceļim jōbyut relatīvim — projektam jōbyut pōrnasamam storp datorim. Nav ļauts izmontōt kodētus absolūtus ceļus.
- Naizmontojit pip atkarību cauruļvada *kodola* modulī. Opcionalōs stadijōs var izmontōt opcionalas pakotnes, un tōm ir graciozi jōdegradējās bez tōm.
- Na pavōjinojiet tikai uz prīkšu vērstū stōvūkļa mašīnu — tys ir izmoksu grīsti.
- Naizmontojit oficiālas ASV valdības atpazeišonas zīmes un na pīvīnojit niku, kas atceļ ōlūta redakcijas.
- D1 shēmas izmaiņas skar **divus** failus: `pipeline/lib/manifest_schema.sql` un `db/schema.sql`.
- Testi ar jaunu kodu. Conventional-commit zinojumi.

Pyrms atvērt PR ar strukturōlom izmaiņom, pyrms tam izlasit `CLAUDE.md` un `docs/20260511/00-*`, tod atverit problēmu pīteikumu, lai apsprīstu.

