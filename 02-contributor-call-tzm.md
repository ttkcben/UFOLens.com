# GitHub — Aselqan 2 seg 3 · Tifrat n inezdaɣ / "isefka yelhan i yimezwura"

**Aseqdac:** Isentel yettwaqerḍen ("Tifrat & isefka yelhan i yimezwura") neɣ aglam n CONTRIBUTING.md.
**Awalen isennanen:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Isemtelen:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Tifrat ɣer ufolens.com

[ufolens.com](https://www.ufolens.com) yettasuffeɣ-d ammud n [PURSUE UAP archive](https://www.war.gov/ufo) n U.S. War Department d aɣbalu yettnadiyen, isgadanen deg waṭas n tutlayin s [public API](https://www.ufolens.com/api/v1). D sin n imuren — local Python ingest pipeline (`pipeline/`) d TypeScript/Hono edge app (`worker/`) — ttemlilit deg yiwet n tfelwit: a bundle n SQL + assets yettwassuffɣen.

Ur tḥwaǧed ara isemlalen n cloud i tifrat. Core modules n pipeline d stdlib-only yerna Worker tests ttamɣen deg umḍiq n tẓeɣ.

### Amɣaṛ

```bash
# pipeline
python3 -m pytest pipeline/tests/          # ilaq ad ilin d yizegzawen meṛṛa, ur yettḥwaǧ ara pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Amkan ideg tella tallalt tameqqrant

**i18n / localization** — `worker/src/i18n/ui-strings.json` d aɣbalu n izamulen n UI. Aseggas n tutlayt s yiwen n wemdan yettalas-d aswir meqqren: ad yettwasbedd asuffeɣ asensli n umuddir, ad yettwasbedd RTL/layout issues, ad yettwasbedd language-negotiation edge cases.

**OCR quality** — asensel n tẓeɣ n isemlalen isemḍelen n yimukan uqbel OCR; aseggas n tmuɣli yettalsen open-source engine d Tesseract fallback deg isemlalen n tmuɣli.

**Accessibility** — audit n isemlalen yettwasnulfun (`worker/src/render/`) mgal WCAG; CSP d tight (ur yelli `unsafe-inline`), dɣa isegzayen ilaq ad amɣen deg-s.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — ugar n igzayen n tẓeɣ, aseggas n uḥakim yelhan, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` d aɣbalu). Asuqqel n design docs ɣer Taglizit marḥaba yis-s.

### Iseḍfa iɣezfanen

- Meṛṛa isentel d isumden — amahil ilaq ad yettwanes s tẓeɣ deg yal imukan. Ur yelli ara isentel n umuddir.
- Ur tenniḍ ara pip dependency ɣer pipeline *core* module. Isentel n tẓeɣ yezmer ad yettwabeddlen s tẓeɣ, yerna ilaq ad yettwamɣel s tẓeɣ mbla-t.
- Ur ttenṭṭeṭ ara asensel n usentel s yiwet n tɣuri — d iswir n iswir.
- Ur tennid ara iferdisen n udabu n U.S., yerna ur tenniḍ ara kra yettnifrin tilɣin yettwaseslen n uɣbalu.
- Ibeddlen n D1 schema ttamɣen **sin** n isemlalen: `pipeline/lib/manifest_schema.sql` d `db/schema.sql`.
- Tests s code nniḍen. Conventional-commit messages.

Γret `CLAUDE.md` d `docs/20260511/00-*` uqbel ad tbeddlen aɣbalu, syin askeɣ-d asentel n wasentel uqbel PR.
