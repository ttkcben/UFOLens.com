# GitHub — Post 2 of 3 · Contributor call / "good first issues"

**Use as:** a pinned Discussion ("Contributing & good first issues") or a CONTRIBUTING.md intro.
**Keywords:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contributing to ufolens.com

[ufolens.com](https://www.ufolens.com) turns the U.S. War Department's [PURSUE UAP archive](https://www.war.gov/ufo) into a searchable, multilingual platform with a [public API](https://www.ufolens.com/api/v1). It's two halves — a local Python ingest pipeline (`pipeline/`) and a TypeScript/Hono edge app (`worker/`) — meeting at one interface: a published SQL + assets bundle.

You don't need any cloud credentials to contribute. The pipeline's core modules are stdlib-only and the Worker tests run against in-memory storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Where help is most useful

**i18n / localization** — `worker/src/i18n/ui-strings.json` is the source of UI strings. Native-speaker review of any non-English locale is high-value: catch awkward machine output, fix RTL/layout issues, improve language-negotiation edge cases.

**OCR quality** — better pre-processing of old typewritten scans before OCR; evaluation harness comparing the open-source engine vs. the Tesseract fallback on sample pages.

**Accessibility** — audit the rendered pages (`worker/src/render/`) against WCAG; the CSP is strict (no `unsafe-inline`), so solutions must work within that.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — more graceful-degradation paths, better progress reporting, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` is the index). Translations of the design docs to English are welcome.

### Ground rules

- All paths relative — the project must be portable across machines. No hardcoded absolute paths.
- Don't add a pip dependency to a pipeline *core* module. Optional stages may use optional packages, and must degrade gracefully without them.
- Don't weaken the forward-only state machine — that's the cost ceiling.
- Don't introduce official U.S. government insignia, and don't add anything that reverses source redactions.
- D1 schema changes touch **two** files: `pipeline/lib/manifest_schema.sql` and `db/schema.sql`.
- Tests with new code. Conventional-commit messages.

Read `CLAUDE.md` and `docs/20260511/00-*` first, then open an issue to discuss anything structural before the PR.
