# GitHub — Fasnin ya 2 ye 3 · Naong-kaagre / "dood fãa yikri"

**Tũu Woto:** Naong-kaagre (“Naong-kaagre la dood fãa yikri”) walla CONTRIBUTING.md.
**Wɛɛg-yõdo:** open source, naong-kaagre, dood fãa yikri, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Teeb-yikri:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Naong-kaagre n tũud ufolens.com

[ufolens.com](https://www.ufolens.com) pids War Department (U.S.) [PURSUE UAP sɛb-yõdrã](https://www.war.gov/ufo) n ya gom-gom pɛlɛg-yãkda, gom-gom tũudma ne [public API](https://www.ufolens.com/api/v1). Kuli ifipande fibili — local Python ingest pipeline (`pipeline/`) na TypeScript/Hono edge app (`worker/`) — filakumana pa interface imo: a published SQL + assets bundle.

Ka yĩnde n gũ-da cloud credentials n tũud. Pipeline core modules sha stdlib-only kabili Worker tests ipita pa in-memory storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Laasg-gomdã ka yikda

**i18n / localization** — `worker/src/i18n/ui-strings.json` e source ya UI strings. Native-speaker review of any non-English locale is high-value: catch awkward machine output, fix RTL/layout issues, improve language-negotiation edge cases.

**OCR quality** — ukunonsha ukulenga bwino kwa old typewritten scans libela OCR; evaluation harness ukulinganisha open-source engine ne Tesseract fallback pa sample pages.

**Accessibility** — ukulanga rendered pages (`worker/src/render/`) ku WCAG; CSP kuli strict (takwaba `unsafe-inline`), nso ifya kulungika filabomba mu yo.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — ukwaba graceful-degradation paths ishingi, ukubilisha bwino kwa progress, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` e index). Ukupilibula kwa design docs ku English kwapokelelwa.

### Gom-gom-minungã

- Ifiluba fyonse relative — umulimo ufwaya ukwaluka pa michini yonse. Takwaba hardcoded absolute paths.
- Ka yĩnde n gũ-da pip dependency ku pipeline *core* module. Optional stages fingabomfya optional packages, kabili filafwaya ukulalala bwino nga takwaba.
- Ka yĩnde n gũ-da forward-only state machine — iyo e cost ceiling.
- Ka yĩnde n gũ-da official U.S. government insignia, kabili takulekwa ukweka ifyacinchika ifyapilibula source redactions.
- D1 schema changes ifibomba pa **mafunde yabili**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Tests ne code impya. Conventional-commit messages.

Palenjeni ukubelenga `CLAUDE.md` na `docs/20260511/00-*` libela, lyena shiluleni issue ku kulandapo ifya mu kuilenga libela PR.

