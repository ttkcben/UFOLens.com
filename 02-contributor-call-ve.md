# GitHub — Khasi ya 2 ya 3 · U vhidza vhaṱanganedzi / "zwithu zwavhuḓi zwa u thoma"

**Shumisa sa:** Discussion yo tivhwaho ("U ṱanganedza & zwithu zwavhuḓi zwa u thoma") kana CONTRIBUTING.md intro.
**Maipfi a ndeme:** open source, u ṱanganedza, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Zwiṱhuṱhisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## U ṱanganedza kha ufolens.com

[ufolens.com](https://www.ufolens.com) i shandula [vhulungelo ha PURSUE UAP](https://www.war.gov/ufo) ha Muhasho wa Nndwa wa U.S. kha thalafurwe i konaho u ṱoḓwa, ya mimuthu yo fhambanaho i re na [public API](https://www.ufolens.com/api/v1). Ndi zwipindwana zwivhili — pipeline ya u ingedza ya Python ya henefho (`pipeline/`) na app ya TypeScript/Hono edge (`worker/`) — zwi ṱanganaho kha interface nthihi: SQL yo phablishiwaho + assets bundle.

A zwi ṱoḓi cloud credentials dza u ṱanganedza. Core modules dza pipeline ndi stdlib-only nahone Worker tests zwi shanda kha in-memory storage.

### U vhea zwithu zwavhuḓi

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Hune thuso ya vha ya ndeme

**i18n / localization** — `worker/src/i18n/ui-strings.json` ndi tshitalo tsha UI strings. U vhaleswa nga muambaleli wa luambo naho lu lufhi lwe lu si lwa English ndi ha ndeme: u vhala machine output yo hulelaho, u lungisa RTL/layout issues, u khwinisa language-negotiation edge cases.

**OCR quality** — u lungisa zwavhuḓi scans dza khorona dzi re hone vhanzhi vha sa athu u shandisa OCR; evaluation harness i tshi ṱanḓamedza open-source engine vs. Tesseract fallback kha sample pages.

**Accessibility** — u sedza rendered pages (`worker/src/render/`) kha WCAG; CSP yo hulelaho (a hu na `unsafe-inline`), ngauralo zwiko zwi fanela u shanda vhukati hazwo.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — dziṅwe dzi nzila dza u thuthea zwavhuḓi, u divhadza mushumo zwavhuḓi, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ndi index). U ṱalutshedzela design docs kha English zwi ṱanganedziwa.

### Milayo ya ndeme

- Dzinzila dzoṱhe dzo linganaho — tshumikiso i fanela u kona u dzulisa kha mimishini yo fhambanaho. A hu na dzinzila dzo hardcodedzwa.
- U sa engedza pip dependency kha pipeline *core* module. Optional stages dzi nga shumisa optional packages, nahone dzi fanela u thuthea zwavhuḓi dzi si na dzona.
- U sa fhedza state machine ya u ya phanḓa fhedzi — yeneyi ndi yone cost ceiling.
- U sa engedza official U.S. government insignia, na u sa engedza naho tshi mini tshi phethedzelaho source redactions.
- D1 schema changes dzi kwama **zwikhala zwivhili**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Tests dza khodo ntswa. Conventional-commit messages.

Vhalani `CLAUDE.md` na `docs/20260511/00-*` u thoma, nga murahu ni bule issue ya u khasedza naho tshi mini tshi mangandavhelo vhanzhi vha sa athu u PR.

