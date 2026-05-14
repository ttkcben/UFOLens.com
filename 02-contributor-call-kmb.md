# GitHub — Dixi 2 dia 3 · Kubwida kwa bantu bakudisa / "mambulu ma mbutu mambote"

**Sadisa kala:** Makani masokeka ("Kubwida na mambulu ma mbutu mambote") mba introdução ya CONTRIBUTING.md.
**Tanga yakwiza:** open source, kubwida, mambu ma mbutu ma mbote, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kubwida ku ufolens.com

[ufolens.com](https://www.ufolens.com) yavula dibulu dia [PURSUE UAP archive](https://www.war.gov/ufo) dia Departementu ya Kialwa ya EUA mu disu disaka, dia laka ya zungulu na [API yabula](https://www.ufolens.com/api/v1). Diba dia madi: pipeline ya ingest ya Python ya dibulu (`pipeline/`) na app ya TypeScript/Hono edge (`worker/`) — mikweti ku interface umosi: bundle ya SQL + assets yabula.

Kana watsakila cloud credentials ku kubwida. Core modules ya pipeline stdlib-only na Worker tests misala ku in-memory storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Apo ubwidi wacindama

**i18n / localization** — `worker/src/i18n/ui-strings.json` e kifunkula dia UI strings. Kuyanda kwa native-speaker ku laka kaykweti Ingileza kuli na valo ya ngolo: kubwida ifyacikidi fya ngalu ifyakola, kulungisa mambulu ma RTL/layout, kukwiza edge cases ya laka ya kubwika.

**OCR quality** — kuyanda kusingila kwa scans za tanga zakulu libwidi libwidi OCR; evaluation harness yakweti kuyanda ya open-source engine na Tesseract fallback ku sample pages.

**Accessibility** — audit ya rendered pages (`worker/src/render/`) ku WCAG; CSP kuli ngolo (kana `unsafe-inline`), nso ifyakuwana bisala mu oyo.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — ngalu za graceful-degradation za ngolo, kupanga kwa progress kwa ngolo, edge cases ya delta-detection (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` e index). Kumanyisa kwa mikanda ya design ku Ingileza kumiabwidi.

### Masumu ma ngolo

- Ifilubu fiyoso relative — nkalu ifwayidi kuyenda ku michini ya yoso. Kana hardcoded absolute paths.
- Kana yakwiza pip dependency ku pipeline *core* module. Stages za nsokela zingasadisa optional packages, na zafwayidi kukolama bwino kana zibwidi.
- Kana yakolama ngalu ya state machine ya kuyenda ku ntwala — oyo e cost ceiling.
- Kana yakwiza official U.S. government insignia, na kana yakwiza kima kimosi kiyana kufungula redakisa za kifunkula.
- D1 schema changes miatala **mikanda mibadi**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Tests na code ya yika. Conventional-commit messages.

Bala `CLAUDE.md` na `docs/20260511/00-*` libwidi libwidi, lyena vula issue ku kwakana kima kimosi kya ngalwa libwidi PR.

