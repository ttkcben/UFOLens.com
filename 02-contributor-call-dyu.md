# GitHub — Jɛɛ 2/3 · Kulu-mɔgɔ weleli / "baara fɔlɔ ɲumanw"

**A kɛcogo:** Kulekan sinsinnen ("Dɛmɛ ni baara fɔlɔ ɲumanw") walima CONTRIBUTING.md kunnatɛ.
**KUMA KƐRƐNKƐRƐNNENW:** open source, dɛmɛ, baara fɔlɔ ɲuman, i18n, sigida-ladamuni, OCR, Python, TypeScript, Vitest, pytest, don-ko ɲuman, UAP, data bɛɛ la
**Jɛgɛrɛw:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Dɛmɛ kɛli ufolens.com la

[ufolens.com](https://www.ufolens.com) bɛ Ameriki ka Kɛlɛ Minisiriso ka [PURSUE UAP sɛbɛnmarayɔrɔ](https://www.war.gov/ufo) kɛ platfɔmu ye min bɛ ɲinini, kan caman na, ni [API bɛɛ la](https://www.ufolens.com/api/v1). A ye fɛn fila ye — Python sara-pipeline sigiyɔrɔ (`pipeline/`) ni TypeScript/Hono edge app (`worker/`) — u bɛ ɲɔgɔn sɔrɔ yɔrɔ kelen na: SQL + nafolo bɔli.

I tɛna kɔlɔsi si mago don ka dɛmɛ kɛ. Pipeline ka modulu fɔlɔw ye stdlib-only ye, wa Worker tɛsitɛriw bɛ baara kɛ memory kɔnɔ marayɔrɔ la.

### Sigi

```bash
# pipeline
python3 -m pytest pipeline/tests/          # a bɛɛ ka kan ka kɛ jiri ye, pip si ma sigi

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Dɛmɛ ka fisa yɔrɔ minnu na

**i18n / sigida-ladamuni** — `worker/src/i18n/ui-strings.json` de ye UI kumaw bɔyɔrɔ ye. Angilɛkan-tɛ-mɔgɔ si ka kan min tɛ Angilɛkan ye, o lajɛli ka bon kosɛbɛ: masini ka bɔli min tɛ ɲɛ, o minɛ, RTL/jɔcogo problemuw ɲɛnabɔ, kan-minɛcogo ka problemuw ɲɛnabɔ.

**OCR kalite** — sɛbɛn kɔrɔw minnu sɛbɛnnen don masini kan, olu labɛn ka ɲɛ sanni OCR ka kɛ; tɛsitɛri min bɛ open-source engine ni Tesseract fallback paragrafi kan pahin misaliw kan.

**Don-ko ɲuman** — pahin minnu bɔra (`worker/src/render/`), olu lajɛ WCAG la; CSP ka gɛlɛn (a tɛ `unsafe-inline` kɛ), o la, furaw ka kan ka baara kɛ o kɔnɔ.

**API ergonomi** — `worker/src/routes/` — pahin-faranfasi, filɛri, OpenAPI lakali, misali kiliyanw.

**Pipeline ka barika** — jɔli-ɲuman-cogo caman, napɔrɔ-di-cogo ka fisa, delta-yecogo ka problemuw (`pipeline/lib/delta.py`).

**Sɛbɛnw** — `docs/20260511/` (繁體中文; `00-*` de ye sɛbɛn ye). Jɔcogo sɛbɛnw ladamuninen Angilɛkan na, olu bɛ jaabi.

### Sariyaw

- Sira bɛɛ ye jɛgɛrɛ ye — porojɛ ka kan ka se ka wuli ka bɔ masini kelen na ka taa wɛrɛ la. Sira absoluta si ma kɛ.
- Pip jɛgɛrɛn si ma fara pipeline *fɔlɔ* modulu kan. Waati opsyonɛliw bɛ se ka pake opsyonɛliw kɛ, wa u ka kan ka jɔ ka ɲɛ n’u tɛ yen.
- Kɔfɛ-filɛli masini min bɛ kɔfɛ-filɛ, o kana barika dɔgɔya — o de ye wari danfɛ ye.
- Ameriki fanga ka taamasiyɛn si ma fara a kan, wa foyi ma fara a kan min bɛ sɛbɛn fɔlɔw majigilenw kɔsegi.
- D1 jɔcogo yɛlɛmaw bɛ dosiye **fila** de sɔrɔ: `pipeline/lib/manifest_schema.sql` ni `db/schema.sql`.
- Tɛsitɛriw ni kodi kura. Conventional-commit kunnafonisɛbɛnw.

`CLAUDE.md` ni `docs/20260511/00-*` kalan fɔlɔ, o kɔ, problemu da wuli ka waari kɛ jɔcogo si kan sanni PR ka kɛ.

