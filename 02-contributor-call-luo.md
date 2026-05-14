# GitHub — Gikwanyisi mar 2 kuom 3 · Luo mar Gikmanyo / "good first issues"

**Ti kaka:** Kaka Twak ma ochung' motegno ("Contributing & good first issues") kata weche manie intro mar CONTRIBUTING.md.
**Weche Mokonyo:** open source, konyo, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Wanjruok mag Kony:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Konyo e ufolens.com

[ufolens.com](https://www.ufolens.com) loko gik mang'ado siri mag U.S. War Department mar [PURSUE UAP archive](https://www.war.gov/ufo) obedo pulatifom ma inyalo manyo godo gik mang'eny, ma nigi dhok mang'eny, kendo ma nigi [public API](https://www.ufolens.com/api/v1). En gir-achiel ariyo — pipeline mar Python mar gweng' (`pipeline/`) kod app mar TypeScript/Hono edge (`worker/`) — ginyomre e interface achiel: bundle mar SQL + assets ma omieng'ore.

Ok dwarore ni ibed gi cloud credentials moro amora mondo ikony. Core modules mag pipeline gin stdlib-only kendo Worker tests timore gi in-memory storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kama konyore ahinya

**i18n / localization** — `worker/src/i18n/ui-strings.json` en kama konyore dhok duto mag UI. Ji duto ma giwacho dhok moro amora ma ok en Ingereza nyalo konyo e manyo gik manyien, gik ma ok ochomo, kata gik ma ok ochomo e dhok moro amora.

**OCR quality** — manyo gik manyien e gik ma ne osegoloe machon ka pok ochieng'ore gi OCR; manyo gik manyien e engine mar open-source vs. Tesseract fallback e gik manyien.

**Accessibility** — manyo gik manyien e page ma ne osegoloe (`worker/src/render/`) vs. WCAG; CSP nigi gi `unsafe-inline`, kendo gik manyien duto nyaka timre e gweng' mar OCR.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — gik manyien ma ok ochomo, gik manyien ma ok ochomo, gik manyien ma ok ochomo (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` en index). Translations mag docs mag design to English oyie.

### Chik duto

- Gik duto manie iye — puojectni nyaka nyalo dhi e masin duto. Ok onyal bedo gi gik manyien ma ok one gi wang' apaa.
- Ok onyal bedo gi pip dependency e pipeline *core* module. Gik manyien duto nyalo bedo gi gik manyien, kendo nyaka bedo gi gik manyien ma ok ochomo.
- Ok onyal bedo gi state machine ma ok ochomo — en kama gik duto ochor godo.
- Ok onyal bedo gi ranyisi moro amora mar sirkal mar U.S., kendo ok onyal bedo gi gik manyien ma ok one gi wang' apaa.
- D1 schema changes otin'g'o fail ariyo: `pipeline/lib/manifest_schema.sql` kod `db/schema.sql`.
- Tests gi code manyien. Conventional-commit messages.

Yiere `CLAUDE.md` kod `docs/20260511/00-*` mokwongo, kendo chop issue mondo ojawuok gi gik manyien ka pok oyie.

