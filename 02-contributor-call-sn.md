# GitHub — Chinyorwa che 2 pazvitatu · Kudanwa kwevabatsiri / "issues yakanaka yekutanga"

**Shandisa se:** Discussion yakapinwa ("Contributing & good first issues") kana nhanganyaya ye CONTRIBUTING.md.
**Keywords:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kubatsira pa ufolens.com

[ufolens.com](https://www.ufolens.com) inochinja [PURSUE UAP archive](https://www.war.gov/ufo) ye U.S. War Department kuva platform inotsvaka inotaura mitauro yakawanda ine [public API](https://www.ufolens.com/api/v1). Inowanikwa muzvikamu zviviri — local Python ingest pipeline (`pipeline/`) ne TypeScript/Hono edge app (`worker/`) — zvinosangana pa interface imwe: bundle ye SQL + assets yakapubliciwa.

Hauudiye cloud credentials dzose kuti ubatsire. Core modules dze pipeline dzinoshandisa stdlib chete uye Worker tests dzinomhanya pa in-memory storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Pane panobatsirwa zvakanyanya

**i18n / localization** — `worker/src/i18n/ui-strings.json` ndiko kwakabva UI strings. Kuongorora kwevashandi vanoziva mutauro (native speakers) kwemitauro isiri English kune kukosha kukuru: kubata zvinobuda muchina zvisina kunaka, kugadzirisa RTL/layout issues, nekuvandudza language-negotiation edge cases.

**OCR quality** — pre-processing iri nani re scans dzekare dzekutype-ira musati maita OCR; evaluation harness inonyatsoenzanisa open-source engine ne Tesseract fallback pama sample pages.

**Accessibility** — kuongorora (audit) mapeji akagadzirwa (`worker/src/render/`) zvichienderana ne WCAG; CSP yakasimba (hapana `unsafe-inline`), saka mazano anofanira kushanda mukati meizvi.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, uye example clients.

**Pipeline robustness** — nzira dze graceful-degradation dzakawanda, kuzivisa kwekubudirira (progress reporting) kuri nani, uye delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ndiyo index). Kushandura design docs kuenda ku English kunogamuchirwa.

### Mitemo yekutanga

- Nzira (paths) dzose relative — project dzinofanira kukwanisa kushandiswa mumashini akasiyana. Hapana hardcoded absolute paths.
- Usawedzera pip dependency ku module ye *core* ye pipeline. Stages dzinogona kusarudzwa (optional) dzinogona kushandisa optional packages, uye dzinofanira kushanda zvakanaka kunyangwe pasina izvozvo.
- Usaderedze simba re forward-only state machine — nekuti ndiyo cost ceiling.
- Usaisa zviratidzo zviri pamutemo zvehurumende ye U.S., uye usawedzera chero chinhu chinodzorera source redactions.
- Kuchinjika kwe D1 schema kunobata mafayira **maviri**: `pipeline/lib/manifest_schema.sql` ne `db/schema.sql`.
- Tests nemakodhi matsva. Conventional-commit messages.

Verengai `CLAUDE.md` ne `docs/20260511/00-*` kutanga, zvozoita vhurai issue kuti mukurukure chero chinhu che structural kuzoti PR.