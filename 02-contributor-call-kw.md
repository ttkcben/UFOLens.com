# GitHub — Post 2 a 3 · Galow dhe geworra / "kudynnow da kynsa"

**Devnydh avel:** Keskows pynnys ("Keworra & kudynnow da kynsa") po komendyans dhe CONTRIBUTING.md.
**Geryow alhwedh:** mammen-yger, keworra, kudyn da kynsa, i18n, leelaans, OCR, Python, TypeScript, Vitest, pytest, hedradowder, UAP, data yger
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Keworra dhe ufolens.com

[ufolens.com](https://www.ufolens.com) a dreyl arhive PURSUE UAP a Ranngylgh Vresel an S.U. [PURSUE UAP archive](https://www.war.gov/ufo) dhe vos platform hwithradow, lies-yethek gans [API poblek](https://www.ufolens.com/api/v1). Yma diw hanter — piblinell-yngesta Python leel (`pipeline/`) hag app amal TypeScript/Hono (`worker/`) — ow metya war unn ynterfas: bondel SQL + asedhow dyllys.

Ny'th esowgh kummyas kommol rag keworra. Modylow kres an biblinell yw stdlib-hepken ha provow an Worker a rol erbynn stokkas yn-kov.

### Setyans

```bash
# pipeline
python3 -m pytest pipeline/tests/          # res porres bos oll gwer, hep esow `pip install`

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Le mayth yw gweres an moyha dhe les

**i18n / leelaans** — `worker/src/i18n/ui-strings.json` yw mammen an kordenow UI. Askwythyans gans kowser genedigek a neb leel nag yw Sowsnek yw a bris meur: dalhenna eskor jynn klock, ewna kudynnow RTL/kemusur, ha gwellhe an kasys amal a gesvreusyans yeth.

**Kwalita OCR** — gwell rag-obera a skandyansow koth skrifys-jynn kyns OCR; harnays-evalua ow keheveli an jynn mammen-yger erbynn an Tesseract a-dhelergh war folennow sampel.

**Hedradowder** — arbrofa an folennow renderrys (`worker/src/render/`) erbynn WCAG; an CSP yw strick (nyns eus `unsafe-inline`), ytho an diskwedhyansow a res oberi a-ji dhe henna.

**Ergonomek an API** — `worker/src/routes/` — folennans, sidhla, deskrivans OpenAPI, klientow ensampel.

**Krevder an biblinell** — moy a hensyow dasgrad-jentyl, gwell menegyans a-dro dhe vrok, kasys amal a dhiskudhans-delta (`pipeline/lib/delta.py`).

**Dokumentow** — `docs/20260511/` (繁體中文; `00-*` yw an endeks). Treylyansow a'n dokumentow desin dhe Sowsnek yw wolkom.

### Regennow an leur

- Oll an hensyow a dal bos kembarlek — res yw dhe'n ragdres bos gwayadow dres jynnow. Nyns eus hensyow kales-kodys absolut.
- Na geworr kreghenson `pip` dhe vodyul *kres* a'n biblinell. Gradhow dewisadow a yll devnydhya pakkys dewisadow, ha res yw dhedha dasgradya'n jentyl hepdha.
- Na wannha an jynn studh rag-onlys — henna yw an nans kost.
- Na geworr arwodhow sodhogel a wovernans an S.U., ha na geworr travyth a dreyl digelennow an vammen.
- Chanjyow dhe skema D1 a doch **dew** fyll: `pipeline/lib/manifest_schema.sql` ha `db/schema.sql`.
- Provow gans kod nowydh. Messajys conventional-commit.

Lenn `CLAUDE.md` ha `docs/20260511/00-*` kynsa, hag ygeri kudyn rag keskewsel a-dro dhe dra vyth framweythek kyns an PR.

