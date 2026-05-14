# GitHub — Mwandĩko wa 2 wa 3 · Kũruta mũtĩrĩria / "njũngwa njega cia mbere"

**Kũhũthĩra ta:** Kwĩrangĩria kwĩ kũrũmĩtwo ("Kũtĩrĩria & njũngwa njega cia mbere") kana gĩcunjĩ kĩa CONTRIBUTING.md.
**Ciugo cia bata:** open source, kũtĩrĩria, njũngwa njega cia mbere, i18n, kũhũthĩra kũrĩa kũrĩ, OCR, Python, TypeScript, Vitest, pytest, kũhũthĩra, UAP, data ĩtarĩ ya hitho
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kũtĩrĩria ufolens.com

[ufolens.com](https://www.ufolens.com) nĩ ngũĩrĩra rũma rwa U.S. War Department's [PURSUE UAP archive](https://www.war.gov/ufo) kũruta platform ĩrĩ kũthũũranĩka, ĩrĩ na ndimi nyingĩ na [public API](https://www.ufolens.com/api/v1). Nĩ icunjĩ igĩrĩ — Python ingest pipeline ya mũciĩ (`pipeline/`) na TypeScript/Hono edge app (`worker/`) — kũgũra interface ĩmwe: SQL + assets bundle ĩrutĩtwo.

Ndũrabatara cloud credentials o na iharĩka nĩ ũndũ wa kũtĩrĩria. Mũthingi wa pipeline nĩ stdlib-only na Worker tests nĩ ngũĩrĩra kũrĩa kũrĩ in-memory storage.

### Kũrũgamĩrĩra

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kũrĩa ũtĩrĩria ũrĩ wa bata mũno

- i18n / kũhũthĩra kũrĩa kũrĩ — `worker/src/i18n/ui-strings.json` nĩ kĩhumo kĩa UI strings. Kũrora kwa mũndũ wa rũthiomi rũrĩa rũtarĩ rwa Gĩthũngũ nĩ gwa bata mũno: kũruta output ĩrĩ ya macini, kũrũmia njũngwa cia RTL/layout, kũrora njũngwa cia kũcũrania rũthiomi.
- Ũthaka wa OCR — kũrora nĩ ũndũ wa kũcũrania gĩthita kĩa typewritten scans mbere ya OCR; evaluation harness kũgeria open-source engine vs. Tesseract fallback kũrĩa sample pages.
- Kũhũthĩra — kũrora rendered pages (`worker/src/render/`) kũrĩa WCAG; CSP nĩ ngugu (gũtirĩ `unsafe-inline`), nĩ ũndũ wa ũrĩa njũngwa cĩa bata kũruta thĩinĩ wa ũrĩa.
- API ergonomics — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.
- Mũthingi wa pipeline — njũngwa njega cia kũhũthĩra, kũrutithia njũngwa njega, delta-detection edge cases (`pipeline/lib/delta.py`).
- Marũa — `docs/20260511/` (繁體中文; `00-*` nĩ index). Gũcũrania marũa ma kũcũrania kũrĩa Gĩthũngũ nĩ ngũĩrĩra.

### Mĩhaka ya mũthingi

- Njũngwa cĩa paths mothe — wĩra ũrĩa ũrĩ bata kũruta thĩinĩ wa macini. Gũtirĩ hardcoded absolute paths.
- Ndũkaingĩrie pip dependency kũrĩa pipeline *core* module. Optional stages nĩ ngũĩrĩra optional packages, na nĩ ngũĩrĩra kũhũthĩra njũngwa njega cia kũhũthĩra.
- Ndũkaingĩrie forward-only state machine — ũrĩa nĩ gĩthĩtĩra kĩa kĩbata.
- Ndũkaingĩrie official U.S. government insignia, na ndũkaingĩrie kĩndũ kĩa kũcoka gũthathaũra maũndũ marutĩtwo.
- D1 schema changes nĩ ngũĩrĩra **marũa igĩrĩ**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Tests na code njerũ. Conventional-commit messages.

Thoma `CLAUDE.md` na `docs/20260511/00-*` mbere, ũndĩ gũgũra issue nĩ ũndũ wa kũrangĩria kĩndũ kĩa mũthingi mbere ya PR.

