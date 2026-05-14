# GitHub — Post 2 sur 3 · Dɛmɛtɔw weleli / "baara fɔlɔw"

**A baara kɛ i n’a fɔ:** pinned Discussion ("Dɛmɛ ni baara fɔlɔw") walima CONTRIBUTING.md daɲɛ fɔlɔw.
**Daɲɛw:** open source, dɛmɛ, baara fɔlɔ, i18n, yɔrɔ-yɔrɔ, OCR, Python, TypeScript, Vitest, pytest, se sɔrɔ, UAP, data dafalen
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ka dɛmɛ don ufolens.com la

[ufolens.com](https://www.ufolens.com) bɛ Ameriki ka Kɛlɛ Departeman ka [PURSUE UAP kɔnɔkow](https://www.war.gov/ufo) kɛ ɲinini platfɔɔmu ye, min bɛ se ka kɛ kanw caman na ani [API jama bɛɛ ye](https://www.ufolens.com/api/v1). A ye fɛn fila ye — Python ingest pipeline lokal (`pipeline/`) ani TypeScript/Hono edge app (`worker/`) — minnu bɛ ɲɔgɔn sɔrɔ interface kelen na: SQL + assets bundle min lajɛra.

I tɛna mɔgɔ si ka cloud credentials sɔrɔ ka dɛmɛ don. Pipeline ka core modules ye stdlib-only ye, wa Worker testw bɛ kɛ ka tɛmɛ in-memory storage kan.

### Labɛnni

```bash
# pipeline
python3 -m pytest pipeline/tests/          # bɛɛ ka kan ka kɛ jɛman ye, pip install tɛna kɛ

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Dɛmɛ ka fisa yɔrɔ minnu na

**i18n / yɔrɔ-yɔrɔ** — `worker/src/i18n/ui-strings.json` ye UI daɲɛw fɔlɔ ye. Mɔgɔ minnu bɛ kan minnu fɔ, olu ka lajɛli bɛ kɛ ka tɛmɛ Angilɛkan kan, o ye nafa sɔrɔ: masin bɔli nɔgɔmanw minɛ, RTL/cogo nɔgɔya, ani kanw ɲɔgɔn sɔrɔ cogo nɔgɔya.

**OCR kalite** — sɛbɛnni kɔrɔw minnu sɛbɛnna, olu ka pre-processing ka fisa sanni OCR kɛ; lajɛli min bɛ open-source engine ni Tesseract fallback fara ɲɔgɔn kan page misaliw la.

**Se sɔrɔ** — lajɛli kɛ `worker/src/render/` page-w la ka tɛmɛ WCAG kan; CSP ka gɛlɛn (tɛ `unsafe-inline`), o la, furaw ka kan ka baara kɛ o kɔnɔ.

**API nɔgɔya** — `worker/src/routes/` — pagination, filtering, OpenAPI description, misali clientw.

**Pipeline ka kantigiya** — nɔgɔya wɛrɛw, ɲɛtaa kunnafoni ka fisa, ani delta-detection edge cases (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ye index ye). Design docs ka kalanko Angilɛkan na bɛ jaabi.

### Sariya fɔlɔw

- Jɛgɛrɛ bɛɛ bɛ tali kɛ ɲɔgɔn na — poroje ka kan ka se ka wuli ka tɛmɛ masinw kan. Absolute path si tɛna kɛ.
- A kana kɛ ka pip dɛmɛni wɛrɛ fara pipeline *core* module kan. Stage wɛrɛw bɛ se ka optional packages baara, wa u ka kan ka nɔgɔya u tɛ.
- A kana kɛ ka forward-only state machine nɔgɔya — o ye wari dan ye.
- Ameriki fanga ka taama-shiya si ma fara a kan, wa fɛn si ma fara a kan min bɛ fɔlɔ ka gundo bɔ.
- D1 schema yɛlɛmaw bɛ tali kɛ filen **fila** la: `pipeline/lib/manifest_schema.sql` ni `db/schema.sql`.
- Testw ni code kura. Conventional-commit kunnafoniw.

CLAUDE.md ni `docs/20260511/00-*` kalan fɔlɔ, o kɔ, i ka ɲinini kɛ ka baro kɛ fɛn o fɛn kan sanni PR kɛ.

