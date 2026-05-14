# GitHub — Akwụkwọ 2 n'ime 3 · Ọkpụkpọ òkù maka ndị ntinye aka / "nsogbu mbụ dị mma"

**Jiri dị ka:** Mkparịta ụka a kapịrị ọnụ ("Ntinye aka & nsogbu mbụ dị mma") ma ọ bụ mmalite CONTRIBUTING.md.
**Okwu Igodo:** open source, inye aka, nsogbu mbụ dị mma, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, ohere, UAP, data mepere emepe
**Njikọ:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Inye aka na ufolens.com

[ufolens.com](https://www.ufolens.com) na-atụgharị ebe nchekwa [PURSUE UAP archive](https://www.war.gov/ufo) nke Ngalaba Agha US ka ọ bụrụ ikpo okwu a na-enyocha, nke nwere ọtụtụ asụsụ yana [API ọha](https://www.ufolens.com/api/v1). Ọ bụ akụkụ abụọ — pipeline ntinye Python mpaghara (`pipeline/`) na ngwa TypeScript/Hono dị na nsọtụ (`worker/`) — na-ezukọ n'otu interface: SQL + ngwugwu akụrụngwa e bipụtara.

Ị chọghị nzere igwe ojii ọ bụla iji nye aka. Modul isi pipeline bụ naanị stdlib na ule Worker na-agba ọsọ megide nchekwa n'ime ebe nchekwa.

### Nhazi

```bash
# pipeline
python3 -m pytest pipeline/tests/          # kwesịrị ịbụ akwụkwọ ndụ akwụkwọ ndụ niile, ọ dịghị mkpa ntinye pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ebe enyemaka kacha baa uru

**i18n / localization** — `worker/src/i18n/ui-strings.json` bụ isi iyi nke eriri UI. Nyochaa onye na-asụ asụsụ nke mpaghara ọ bụla na-abụghị Bekee bara uru dị ukwuu: jide mmepụta igwe na-adịghị mma, dozie nsogbu RTL/nhazi, ma melite ikpe mkparịta ụka asụsụ.

**Ogo OCR** — nhazi mbụ ka mma nke nyocha ochie edere tupu OCR; eriri nyocha na-atụnyere injin mepere emepe na Tesseract fallback na ibe nlele.

**Nnweta** — nyochaa ibe ndị a sụgharịrị (`worker/src/render/`) megide WCAG; CSP siri ike (enweghị `unsafe-inline`), yabụ ngwọta ga-arụ ọrụ n'ime nke ahụ.

**API ergonomics** — `worker/src/routes/` — pagination, nyocha, nkọwa OpenAPI, ndị ahịa ihe atụ.

**Nkwụsi ike pipeline** — ụzọ mbelata amara karịa, mkpesa ọganihu ka mma, ikpe nchọpụta delta (`pipeline/lib/delta.py`).

**Akwụkwọ** — `docs/20260511/` (繁體中文; `00-*` bụ ndeksi). A na-anabata ntụgharị akwụkwọ nhazi gaa na Bekee.

### Iwu Mmalite

- Ụzọ niile dị n'otu — ọrụ a ga-ebugharị n'ofe igwe. Enweghị ụzọ zuru oke edobere.
- Atụkwasịla ndabere pip na modul *isi* pipeline. Usoro nhọrọ nwere ike iji ngwugwu nhọrọ, ma ga-eji amara weda ya n'enweghị ha.
- Emebila igwe steeti na-aga n'ihu — nke ahụ bụ elu ụlọ.
- Etinyekwala akara gọọmentị US, ma etinyekwala ihe ọ bụla na-atụgharị mmegharị isi mmalite.
- Mgbanwe atụmatụ D1 na-emetụ faịlụ **abụọ**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Nnwale nwere koodu ọhụrụ. Ozi mgbasa ozi nkịtị.

Gụọ `CLAUDE.md` na `docs/20260511/00-*` mbụ, wee mepee nsogbu iji kwurịta ihe ọ bụla gbasara nhazi tupu PR.
