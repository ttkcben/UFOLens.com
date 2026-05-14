# GitHub — Post 2 ng 3 · Panawagan sa mga kontribyutor / "magandang panimulang isyu"

**Gamitin bilang:** isang naka-pin na Discussion ("Pag-ambag at magandang panimulang isyu") o isang panimula sa CONTRIBUTING.md.
**Keywords:** open source, pag-aambag, magandang panimulang isyu, i18n, lokalisasyon, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Pag-aambag sa ufolens.com

Ginagawa ng [ufolens.com](https://www.ufolens.com) ang [PURSUE UAP archive](https://www.war.gov/ufo) ng U.S. War Department na isang nahahanap, multilinggwal na platform na may [pampublikong API](https://www.ufolens.com/api/v1). Ito ay dalawang hati — isang lokal na Python ingest pipeline (`pipeline/`) at isang TypeScript/Hono edge app (`worker/`) — na nagtatagpo sa isang interface: isang na-publish na SQL + assets bundle.

Hindi mo kailangan ng anumang cloud credentials para mag-ambag. Ang mga core module ng pipeline ay stdlib-only at ang mga pagsubok sa Worker ay tumatakbo laban sa in-memory storage.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # dapat lahat ay berde, walang kinakailangang pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kung saan pinakakapaki-pakinabang ang tulong

**i18n / lokalisasyon** — Ang `worker/src/i18n/ui-strings.json` ang pinagmulan ng mga string ng UI. Malaki ang halaga ng pagsusuri ng sinumang katutubong nagsasalita ng anumang di-Ingles na lokal: mahuli ang mga awkward na machine output, ayusin ang mga isyu sa RTL/layout, pagbutihin ang mga edge case sa negosasyon ng wika.

**Kalidad ng OCR** — mas mahusay na pre-processing ng mga lumang typewritten scan bago ang OCR; evaluation harness na naghahambing sa open-source engine laban sa Tesseract fallback sa mga sample na pahina.

**Accessibility** — i-audit ang mga na-render na pahina (`worker/src/render/`) laban sa WCAG; mahigpit ang CSP (walang `unsafe-inline`), kaya dapat gumana ang mga solusyon sa loob nito.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, paglalarawan ng OpenAPI, mga halimbawang kliyente.

**Katatagan ng pipeline** — mas maraming graceful-degradation path, mas mahusay na pag-uulat ng progreso, mga edge case sa delta-detection (`pipeline/lib/delta.py`).

**Mga Doc** — `docs/20260511/` (繁體中文; ang `00-*` ay ang index). Malugod na tinatanggap ang mga pagsasalin ng mga dokumento ng disenyo sa Ingles.

### Mga pangunahing alituntunin

- Lahat ng path ay relatibo — dapat portable ang proyekto sa iba't ibang makina. Walang hardcoded na absolute path.
- Huwag magdagdag ng pip dependency sa isang pipeline *core* module. Ang mga opsyonal na yugto ay maaaring gumamit ng mga opsyonal na package, at dapat mag-degrade nang maayos nang wala ang mga ito.
- Huwag pahinain ang forward-only state machine — iyon ang cost ceiling.
- Huwag magpakilala ng opisyal na sagisag ng gobyerno ng U.S., at huwag magdagdag ng anumang bagay na nagbabaligtad sa mga source redaction.
- Ang mga pagbabago sa D1 schema ay nakakaapekto sa **dalawang** file: `pipeline/lib/manifest_schema.sql` at `db/schema.sql`.
- Mga pagsubok na may bagong code. Mga mensahe ng Conventional-commit.

Basahin muna ang `CLAUDE.md` at `docs/20260511/00-*`, pagkatapos ay magbukas ng isyu upang talakayin ang anumang bagay na pang-istruktura bago ang PR.

