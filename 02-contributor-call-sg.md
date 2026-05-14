# GitHub — Kiri 2 ti 3 · E-hiri ti zo ti mû maboko / "nzoni a-issue ti komanse"

**Tongana ti sara kua na ni:** mbeni Tokua so a gbian ("Mungo maboko & nzoni a-issue ti komanse") wala mbeni introduction ti CONTRIBUTING.md.
**Atënë ti nda ni:** open source, mungo maboko, nzoni issue ti komanse, i18n, localisation, OCR, Python, TypeScript, Vitest, pytest, accessibilité, UAP, atënë so a yeke senge
**Aroko:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mungo maboko na ufolens.com

[ufolens.com](https://www.ufolens.com) ayeke changé [archive ti PURSUE UAP](https://www.war.gov/ufo) ti U.S. War Department na mbeni plate-forme so a lingbi ti gi yâ ni na ayanga ti kodro mingi so ayeke na mbeni [API so ayeke na gbele azo kue](https://www.ufolens.com/api/v1). A yeke a-partie use — mbeni pipeline ti Python ti ndo ni (`pipeline/`) na mbeni application ti edge ti TypeScript/Hono (`worker/`) — so ayeke tingbi na mbeni interface oko: mbeni paquet ti SQL + a-asset so a fa na gigi.

Mo yeke na bezoin ti mbeni credential ti cloud pëpe ti mû maboko. A-module ti nda ti pipeline ni ayeke stdlib-only na a-test ti Worker ayeke tambela na lege ti mbeni stockage so ayeke na yâ ti mémoire.

### Kozo lege

```bash
# pipeline
python3 -m pytest pipeline/tests/          # a doit ti yeke vert kue, a yeke na bezoin ti pip install pëpe

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ndo so mungo maboko ayeke na ngele mingi

**i18n / localisation** — `worker/src/i18n/ui-strings.json` ayeke nda ti atënë ti UI. Révision ti mbeni zo so ayeke zo ti yanga ti kodro ti mbeni yanga ti kodro so ayeke Anglais pëpe ayeke na ngele mingi: wara atënë ti machine so ayeke sioni, leke a-problème ti RTL/layout, leke a-cas ti négociation ti yanga ti kodro.

**Nzoni ti OCR** — nzoni pré-traitement ti a-scan ti akota machine à écrire kozo ti OCR; harnais ti évaluation so ayeke mû lege ti bâ différence ti moteur open-source na fallback ti Tesseract na ndo ti a-page ti tapande.

**Accessibilité** — bâ a-page so a render (`worker/src/render/`) na lege ti WCAG; CSP ayeke ngangu (`unsafe-inline` pëpe), tongaso a-solution doit ti sara kua na yâ ni.

**Ergonomie ti API** — `worker/src/routes/` — pagination, filtrage, description OpenAPI, a-client ti tapande.

**Ngangu ti pipeline** — alege mingi ti dégradation-gracieuse, nzoni rapport ti progrès, a-cas ti détection-delta (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ayeke index ni). A yeke wara a-traduction ti a-document ti conception na Anglais.

### A-règle ti nda ni

- Alege kue ayeke relatif — kusala ni doit ti yeke portable na popo ti a-machine. Mbeni lege so ayeke absolu so a sû na kode pëpe.
- Zîa mbeni dépendance ti pip na mbeni module ti *nda* ti pipeline pëpe. A-étape so ayeke optionnel a lingbi ti sara kua na a-package so ayeke optionnel, na a doit ti descendre na fason so ayeke nzoni sân ala.
- Sara si machine ti état so ayeke gue gi na li ni adescend pëpe — so ayeke plafond ti futango ye.
- Zîa mbeni insigne officiel ti gouvernement ti États-Unis pëpe, na zîa ye oko so ayeke kiri na peko ti a-redaction ti nda ni pëpe.
- A-changement ti schéma ti D1 ayeke ndu a-fichier **use**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- A-test na mbeni code so ayeke fini. A-message ti conventional-commit.

Diko `CLAUDE.md` na `docs/20260511/00-*` kozo, na pekoni mo lungula mbeni issue ti sara tënë na ndo ti mbeni ye ti structure kozo ti PR.

