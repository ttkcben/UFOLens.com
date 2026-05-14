# GitHub — Icipande 2 pali 3 · Ubukombe bwa bantu bakubomba / "good first issues"

**Cibomfiwe nga:** Ifyalandwe fyatekelwe ("Ukubomba & good first issues") nelyo intulo ya CONTRIBUTING.md.
**Amashiwi akulu:** open source, ukubomba, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ukubomba pa ufolens.com

[ufolens.com](https://www.ufolens.com) ilenga ububungwe bwa U.S. War Department bwa [PURSUE UAP archive](https://www.war.gov/ufo) ukuba icikuulwa cikusangilwako ifyakufwaya, icabela indimi ishingi ne [public API](https://www.ufolens.com/api/v1). Kuli ifipande fibili — a local Python ingest pipeline (`pipeline/`) na a TypeScript/Hono edge app (`worker/`) — filakumana pa interface imo: a published SQL + assets bundle.

Tamulefwaya ifya cloud credentials ku kubomba. Pipeline core modules sha stdlib-only kabili Worker tests ipita pa in-memory storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Apo ubwafwilisho bwacindama

**i18n / localization** — `worker/src/i18n/ui-strings.json` e source ya UI strings. Ukubilisha kwa native-speaker pa non-English locale yonse kwacindama: ukusanga ifyacitika fya muchini ifyashalila, ukulungika RTL/layout issues, ukukusha language-negotiation edge cases.

**OCR quality** — ukunonsha ukulenga bwino kwa old typewritten scans libela OCR; evaluation harness ukulinganisha open-source engine ne Tesseract fallback pa sample pages.

**Accessibility** — ukulanga rendered pages (`worker/src/render/`) ku WCAG; CSP kuli strict (takwaba `unsafe-inline`), nso ifya kulungika filabomba mu yo.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

**Pipeline robustness** — ukwaba graceful-degradation paths ishingi, ukubilisha bwino kwa progress, delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` e index). Ukupilibula kwa design docs ku English kwapokelelwa.

### Ifishinte

- Ifiluba fyonse relative — umulimo ufwaya ukwaluka pa michini yonse. Takwaba hardcoded absolute paths.
- Don't add a pip dependency to a pipeline *core* module. Optional stages fingabomfya optional packages, kabili filafwaya ukulalala bwino nga takwaba.
- Don't weaken the forward-only state machine — iyo e cost ceiling.
- Don't introduce official U.S. government insignia, and don't add anything that reverses source redactions.
- D1 schema changes ifibomba pa **mafunde yabili**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Tests with new code. Conventional-commit messages.

Palenjeni ukubelenga `CLAUDE.md` na `docs/20260511/00-*` libela, lyena shiluleni issue ku kulandapo ifya mu kuilenga libela PR.

