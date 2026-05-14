# GitHub — Post 2 ta 3 · Contributor call / "good first issues"

**A na taa yi kamar:** a pinned Discussion ("Contributing & good first issues") ko CONTRIBUTING.md intro.
**Keywords:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com goono ka taa contribute

[ufolens.com](https://www.ufolens.com) U.S. War Department ta [PURSUE UAP archive](https://www.war.gov/ufo) ka sakiya, searchable, multilingual platform ye [public API](https://www.ufolens.com/api/v1) goono. A halves biyu no — local Python ingest pipeline (`pipeline/`) o TypeScript/Hono edge app (`worker/`) — ka taa meet interface guda goono: published SQL + assets bundle.

Ba ya buqata cloud credentials contribution ga. Pipeline core modules stdlib-only no, o Worker tests in-memory storage goono ka taa run.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Yeru goono ka taa ba da taimako

**i18n / localization** — `worker/src/i18n/ui-strings.json` UI strings taa source no. Native-speaker review non-English locale gaba goono ka taa high-value no: awkward machine output goono ka taa catch, RTL/layout issues goono ka taa fix, language-negotiation edge cases goono ka taa improve.

**OCR quality** — better pre-processing old typewritten scans goono goono OCR ba, evaluation harness open-source engine goono ka taa compare Tesseract fallback goono sample pages goono.

**Accessibility** — audit rendered pages goono (`worker/src/render/`) WCAG goono; CSP strict no (`no unsafe-inline`), to solutions a goono ka taa work.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — more graceful-degradation paths, better progress reporting, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` index no). Translations design docs goono English ga welcome no.

### Ground rules

- All paths relative no — project portable no machines gaba goono ka taa. Ba ya hardcode absolute paths.
- Ba ya add pip dependency pipeline *core* module ga. Optional stages optional packages goono ka taa yi, o ba ya degrade gracefully.
- Ba ya weaken forward-only state machine goono — wande cost ceiling no.
- Ba ya introduce official U.S. government insignia, o ba ya add wande ka reversals source redactions goono.
- D1 schema changes **files biyu** goono ka taa touch: `pipeline/lib/manifest_schema.sql` o `db/schema.sql`.
- Tests code kura goono. Conventional-commit messages.

`CLAUDE.md` o `docs/20260511/00-*` goono ka taa taa, structural changes goono ka taa open goono PR ba.

