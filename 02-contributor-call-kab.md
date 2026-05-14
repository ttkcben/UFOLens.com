# GitHub — Aselqan 2 deg 3 · Aseqqam n umussu / "ibeddel imezwura yelhan"

**Seqdec s:** Yiwen usegzi iṛeẓmen ("Aseqqam d ibeddel imezwura yelhan") neɣ yiwen n udris n uḥerrek n CONTRIBUTING.md.
**Awalen ileqqmen:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Isemtuyen:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Aseqqam ɣer ufolens.com

[ufolens.com](https://www.ufolens.com) tesɛaḥḥar azzlu n [PURSUE UAP archive](https://www.war.gov/ufo) n Weqqas n Tussna n Weɛraq n Marikan ɣer taɣult yezmer ad yettunadi, s waṭas n tutlayin, s yiwen n [API amussu](https://www.ufolens.com/api/v1). D sin n imuren — yiwen n Python ingest pipeline aẓṛi (`pipeline/`) d yiwen n TypeScript/Hono edge app (`worker/`) — ttemlilin deg yiwen n udlif: yiwen n SQL + asset bundle yettwaɛedlen.

Ur teḥwiǧ ara kra n iɛellam n cloud i umussu. Core modules n pipeline d stdlib-only, d test n Worker tttnernint ɣef storage in-memory.

### Tinegwa

```bash
# pipeline
python3 -m pytest pipeline/tests/          # yessefk ad yili d azegzaw amagnu, ur teḥwiǧ ara pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Anda yelha ufus n tallelt

**i18n / localization** — `worker/src/i18n/ui-strings.json` d aẓṛi n uḍris n UI. Azrew n umeslay aẓṛi n yal tutlayt ur nelli ara Taglizit d amagnu: ḥṛez aseqdec n tmunt yettwaɣayen, ḥṛez iɣeblan n RTL/layout, snerni iḥebsiwen n umeslay n tutlayt.

**OCR quality** — pre-processing yelhan n isefka n ttergelt taqdimt uqbel OCR; evaluation harness yettwaɣay s open-source engine vs Tesseract fallback ɣef isefka n isebtar.

**Accessibility** — audit n isebtar yettwaɛedlen (`worker/src/render/`) ɣef WCAG; CSP d aǧehdan (ur yelli ara `unsafe-inline`), dɣa iselmaden yessefk ad ttnarnint deg-s.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — abrid n tɣellist yelhan, abrid n tnernit yelhan, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` d amazzlu). Aseqdec n isefka n umecwar ɣer Taglizit d amagnu.

### Iɣellayen

- Yal abrid amagnu — aheggan yessefk ad yili d amussu ɣef imeslayen. Ur yelli ara abrid aḥesfan n isemẓa.
- Ur tɛeddil ara yiwen n pip dependency ɣer pipeline *core* module. Ibeddel n iḥekmen yezmer ad yesseqdec optional packages, yerna yessefk ad yettwaɣay s tɣellist m’ur yelli ara.
- Ur tɣeṛṛeḍ ara forward-only state machine — d win i d agur n tulay.
- Ur tɛeddil ara isem n uwanak n Marikan, yerna ur tɛeddil ara kra n tɣawsa yessuɣulen asefru.
- Ibeddel n D1 schema ttemlilin **sin** n isefka: `pipeline/lib/manifest_schema.sql` d `db/schema.sql`.
- Iḍrisen s umasay amayn. Conventional-commit messages.

Ad tɣeṛṛeḍ `CLAUDE.md` d `docs/20260511/00-*` uqbel, syin ad teldid yiwen ugur ad tmeslayeḍ ɣef yal imagnu uqbel PR.
