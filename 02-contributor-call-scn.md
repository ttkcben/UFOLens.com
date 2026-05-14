# GitHub — Post 2 di 3 · Chiamata a li cuntribbutura / "boni primi issues"

**Usu comu:** na Discussioni fissata ("Cuntribbuiri & boni primi issues") o n'introduzzioni a CONTRIBUTING.md.
**Paroli chiavi:** open source, cuntribbuiri, bona prima issue, i18n, lucalizzazzioni, OCR, Python, TypeScript, Vitest, pytest, accessibilità, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cuntribbuiri a ufolens.com

[ufolens.com](https://www.ufolens.com) trasforma l'[archiviu PURSUE UAP](https://www.war.gov/ufo) dû Dipartimentu dâ Guerra dî Stati Uniti nta na piattaforma ricircàbbili e multilingui cu n'[API pubbrica](https://www.ufolens.com/api/v1). È fattu di dui menzi — na pipeline d'ingestioni Python lucali (`pipeline/`) e n'app edge TypeScript/Hono (`worker/`) — ca s'incontranu nta n'interfaccia sula: un pacchettu SQL + assets pubblicatu.

Nun v'servi nudda cridenziali cloud pi cuntribbuiri. Li mòduli principali dâ pipeline sunnu sulu-stdlib e li test dû Worker funzionanu contru a na mimoria n-memoria.

### Misa a puntu

```bash
# pipeline
python3 -m pytest pipeline/tests/          # avissi a èssiri tuttu virdi, senza bisognu di pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Unni l'aiutu è cchiù ùtili

**i18n / lucalizzazzioni** — `worker/src/i18n/ui-strings.json` è la surgenti dî stringhi di l'UI. La revisioni di un parranti nativu di qualisiasi lingua non anglisi è di granni valuri: curreggiri output automaticu strambu, riparari prubblemi di RTL/layout, megghiuari casi di cunfini ntâ niguzziazzioni dâ lingua.

**Qualità di l'OCR** — megghiu pre-elabburazzioni di vecchi scanzioni a màchina di scrìviri prima di l'OCR; un sistema di valutazzioni ca cumpàra lu muturi open-source contru lu fallback Tesseract supra pàggini di campioni.

**Accessibilità** — auditari li pàggini rinnuti (`worker/src/render/`) contru li WCAG; lu CSP è rigurusu (nuddu `unsafe-inline`), quinni li soluzzioni hannu a funziunari ntra ssu cuntestu.

**Ergunumìa di l'API** — `worker/src/routes/` — paginazzioni, filtraggiu, discrizzioni OpenAPI, clienti d'esempiu.

**Robustizza dâ pipeline** — cchiù percorsi di degradazzioni grazziusa, megghiu rapportu supra lu prugressu, casi di cunfini ntâ rilevazzioni di delta (`pipeline/lib/delta.py`).

**Ducumenti** — `docs/20260511/` (繁體中文; `00-*` è l'ìnnici). Li traduzzioni dî ducumenti di prugettazzioni n anglisi sunnu benvinuti.

### Rèuli di basi

- Tutti li percorsi rilativi — lu prugettu havi a èssiri purtàbbili ntra diversi màchini. Nuddu percorsu assulutu hardcoded.
- Nun agghiùnciri na dipinnenza pip a un mòdulu *principali* dâ pipeline. Fasi opzionali ponnu usari pacchetti opzionali, e hannu a degradari cu grazia senza di iddi.
- Nun ncumminari la màchina a stati sulu-avanti — chistu è lu tettu massimu di costu.
- Nun agghiùnciri nsigni ufficiali dû guvernu dî Stati Uniti, e nun agghiùnciri nenti ca annulla li redazzioni dâ surgenti.
- Li canciamenti ô schema D1 tuccanu **dui** file: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Test cu còdici novu. Missaggi di commit Conventional-commit.

Liggiti prima `CLAUDE.md` e `docs/20260511/00-*`, poi apri n'issue pi discùtiri qualisiasi cosa strutturali prima dû PR.

