# GitHub — Xiviko 2 hi 3 · Ndzingo wa Vahumelerisi / "issues to simeka hi wona"

**Tirhisa tanihi:** Nkangano lowu khomiweke ("Ku humelerisa & issues to simeka hi wona") kumbe xivandlani xa CONTRIBUTING.md.
**Marito-nkulu:** open source, ku humelerisa, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, data yo pfuriwa
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ku humelerisa eka ufolens.com

[ufolens.com](https://www.ufolens.com) yi hundzula vuhlayiselo bya [PURSUE UAP archive](https://www.war.gov/ufo) bya Ndzawulo ya Nyimpi ya U.S. eka xisekelo lexi kambisisekaka, xa tindzimi to tala na [public API](https://www.ufolens.com/api/v1). I tihakelo timbirhi — pipeline ya Python ya kwala (`pipeline/`) na app ya TypeScript/Hono edge (`worker/`) — leti hlangana eka interface yin'we: a published SQL + assets bundle.

A wu lavi cloud credentials to humelerisa. Core modules ta pipeline i stdlib-only naswona Worker tests ti tirha eka in-memory storage.

### Ku simeka

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Laha mpfuno wu pfuna ngopfu

- **i18n / localization** — `worker/src/i18n/ui-strings.json` i xihlovo xa UI strings. Ku kambisisiwa hi mutsari wa ririmi-ro-tala wa xi-locale lexi nga xinge-Xinghezi i xa nkoka: hloma misava leyi tirhiwaka hi muchini leyi nga ri yona, lulamisa issues ta RTL/layout, antswisa swiyimo swa language-negotiation edge.

- **Vun'wana bya OCR** — pre-processing leyi antswaka ya tikhopi to tsariwa hi muchini ta khale u nga si tirhisa OCR; evaluation harness leyi fananisaka open-source engine na Tesseract fallback eka mapheji ya sample.

- **Accessibility** — kambisisa mapheji lama humesiweke (`worker/src/render/`) hi ku landza WCAG; CSP yi nonon'hwerile (ku hava `unsafe-inline`), hikokwalaho swiletelo swi fanele ku tirha endzeni ka sweswo.

- **API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, example clients.

- **Pipeline robustness** — tindlela to hoxeka hi vuenti leti antswaka, ku vika vutihlamuleri lebyi antswaka, delta-detection edge cases (`pipeline/lib/delta.py`).

- **Matsalwa** — `docs/20260511/` (繁體中文; `00-*` i index). Vuhundzuluxi bya design docs eka Xinghezi swa amukeleka.

### Milawu ya le hansi

- Tindlela hinkwato ti fanele ku va relative — xipimana xi fanele ku famba emichinini yo hambana. Ku hava tindlela ta absolute leti hlamuseriweke hi ku nonon'hwerisa.
- U nga engeteli pip dependency eka pipeline *core* module. Swiyimo swa optional swi nga tirhisa optional packages, naswona swi fanele ku hoxeka hi vuenti handle ka swona.
- U nga tsanisi muchini wa xiyimo xo famba mahlweni ntsena — wolowo i ntengo wa le henhla.
- U nga nghenisi swikombiso swa ximfumo swa mfumo wa U.S., naswona u nga engeteli nchumu lexi tlherisaka swihundla swa xihlovo.
- D1 schema changes ti khumba **tinhlayo timbirhi**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Tests na khodi leyintshwa. Milayeni ya Conventional-commit.

Hlaya `CLAUDE.md` na `docs/20260511/00-*` ximho, kutani u pfula issue to vulavurisana hi nchumu wo vukatsi u nga si endla PR.

