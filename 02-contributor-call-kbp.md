# GitHub — Post 2 of 3 · Nɔɔŋ gbɔŋgɔrtoo / "gbɔŋgɔrtoo tɔm tɩkpɛŋŋŋŋ"

**Tɔzɩ kpee nɛ:** ŋgbannɩ Tɔm ("Contributing & good first issues") yaa CONTRIBUTING.md intro.
**Yaaŋ hɔɔlɩŋ:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Lanaa taa kpaŋŋa:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com gbɔŋgɔrtoo

[ufolens.com](https://www.ufolens.com) lɛ, ɛ-U.S. War Department ɛ-PURSUE UAP archive taa tɔm yɔɔŋ gbɔŋgɔrtoo nɛ kɛlɛkɛlɛ nɛ multilingual platform nɛ public API. Pipeline (`pipeline/`) nɛ TypeScript/Hono edge app (`worker/`) lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo. SQL + assets bundle lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo.

Cloud credentials ɛ-taa tɔm yɔɔŋ gbɔŋgɔrtoo tɩkpɛŋŋŋŋ. Pipeline taa tɔm yɔɔŋ lɛ, stdlib-only nɛ Worker tests lɛ, in-memory storage ɛ-taa tɔm yɔɔŋ gbɔŋgɔrtoo.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Gbɔŋgɔrtoo kɛlɛkɛlɛ

**i18n / localization** — `worker/src/i18n/ui-strings.json` lɛ, UI strings ɛ-taa tɔm yɔɔŋ gbɔŋgɔrtoo. Native-speaker review of any non-English locale lɛ, high-value: awkward machine output, RTL/layout issues, language-negotiation edge cases.

**OCR quality** — pre-processing of old typewritten scans before OCR; evaluation harness comparing the open-source engine vs. the Tesseract fallback on sample pages.

**Accessibility** — rendered pages (`worker/src/render/`) WCAG ɛ-taa tɔm yɔɔŋ gbɔŋgɔrtoo; CSP lɛ, strict (no `unsafe-inline`), so solutions must work within that.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — graceful-degradation paths, better progress reporting, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` is the index). Translations of the design docs to English lɛ, kɛlɛkɛlɛ.

### Ground rules

- All paths relative — project lɛ, portable across machines. No hardcoded absolute paths.
- Don't add a pip dependency to a pipeline *core* module. Optional stages may use optional packages, and must degrade gracefully without them.
- Don't weaken the forward-only state machine — that's the cost ceiling.
- Don't introduce official U.S. government insignia, and don't add anything that reverses source redactions.
- D1 schema changes touch **two** files: `pipeline/lib/manifest_schema.sql` and `db/schema.sql`.
- Tests with new code. Conventional-commit messages.

`CLAUDE.md` nɛ `docs/20260511/00-*` gbɔŋgɔrtoo tɩkpɛŋŋŋŋ, then open an issue to discuss anything structural before the PR.

