# GitHub — Post 2 mwa 3 · Kuitana anthu othandiza / "good first issues"

**Gwiritsani ntchito monga:** Discussion yomwe yakhazikitsidwa ("Contributing & good first issues") kapena chiyambi cha CONTRIBUTING.md.
**Keywords:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kuthandiza pa ufolens.com

[ufolens.com](https://www.ufolens.com) sintha [PURSUE UAP archive](https://www.war.gov/ufo) ya U.S. War Department kukhala platform yomwe ikhoza kufufuzidwa ndipo ili m'zinenero zosiyanasiyana pamodzi ndi [public API](https://www.ufolens.com/api/v1). Ili ndi mbali ziwiri — local Python ingest pipeline (`pipeline/`) ndi TypeScript/Hono edge app (`worker/`) — zomwe zikukumana pa interface imodzi: published SQL + assets bundle.

Sikupezeka necessity ya cloud credentials kuti muthandize. Core modules za pipeline ndi stdlib-only ndipo Worker tests zimayendera pa in-memory storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Pamene thandizo likhala lofunika kwambiri

**i18n / localization** — `worker/src/i18n/ui-strings.json` ndiye gwero la UI strings. Kuunika kwa anthu omwe amalankhula chinenero chochitika (native speakers) pa locale iliyonse yomwe si English ndi yofunika kwambiri: kufotokoza zolakwika za machine output, kukonza RTL/layout issues, ndiponso kukonza language-negotiation edge cases.

**OCR quality** — kukonza bwino pre-processing ya old typewritten scans pamaso pa OCR; evaluation harness yomwe imayezera open-source engine poyerekeza ndi Tesseract fallback pa sample pages.

**Accessibility** — kuunika rendered pages (`worker/src/render/`) poyerekeza ndi WCAG; CSP ndi yoyesera kwambiri (no `unsafe-inline`), choncho njira zothetsera zoyenera kugwira ntchito mkati mwa zimenezo.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — njira zosonyeza kuchepa kwa mphamvu mwachifundo (graceful-degradation paths), reporting ya progress yomwe ili bwino, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ndiye index). Masulidwe a design docs ku English amalandiridwa.

### Malamulo a m'mbuyo

- Njira (paths) zonse relative — project iyenera kukhala portable pakati pa makompyuta. Pasowa hardcoded absolute paths.
- Musawonjezere pip dependency ku pipeline *core* module. Optional stages zitha kugwiritsa ntchito optional packages, ndipo ziyenera kugwira ntchito bwino ngakhale popanda izo.
- Musafooketse forward-only state machine — yomwe ndi cost ceiling.
- Musayambe kugwiritsa ntchito official U.S. government insignia, ndipo musawonjezere chilichonse chomwe chimabwezeretsa source redactions.
- Kusintha kwa D1 schema kumakhudza mafayilo **awiri**: `pipeline/lib/manifest_schema.sql` ndi `db/schema.sql`.
- Tests pamodzi ndi code yatsopano. Conventional-commit messages.

Werengani `CLAUDE.md` ndi `docs/20260511/00-*` choyamba, kenako tsegulani issue kuti mukambirane zinthu za structural pamaso pa PR.