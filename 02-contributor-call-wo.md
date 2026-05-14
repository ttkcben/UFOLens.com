# GitHub — Post 2 of 3 · Contributor call / "good first issues"

**Use as:** a pinned Discussion ("Contributing & good first issues") or a CONTRIBUTING.md intro.
**Keywords:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Jàppale ci ufolens.com

[ufolens.com](https://www.ufolens.com) dafay soppi dalu [PURSUE UAP archive](https://www.war.gov/ufo) bu Departemaŋ Xare bu U.S. bi def ko jumtukaay bu mën a seet, am ay làkk yu bare, ak ab [API bu ubbiku](https://www.ufolens.com/api/v1). Ñaari xaaj la — ab pipeline bu Python bu dugg ci ordinatëer (`pipeline/`) ak ab appu edge bu TypeScript/Hono (`worker/`) — dañuy daje ci benn jongo: ab paket bu SQL + ay resurs yu ñu pibli.

Soppiwuloo benn idantifiyan bu cloud ngir mën a jàppale. Modili cëru pipeline bi dañuy jëfandikoo stdlib rekk te testu Worker yi dañuy dox ci ndëxëñ bu nekk ci memoria.

### Taxawal

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Fu ndimbal li gën a am njariñ

**i18n / lokalisason** — `worker/src/i18n/ui-strings.json` moy cosaanu baat yu nekk ci UI bi. Képp ku làkk wu dul angale sa làkku ndey, sa xool bu am solo la: gis tekki masin yu nox, jural saytu RTL/layout issues, yokk ay cas yu mujj ci waxtaanu làkk.

**Kalite OCR** — gën a waajal skan yu yàgg yu ñu bind ak masin laata OCR; ab armaturu evalyasyon buy méngale moteur bu ubbiku bi ak Tesseract fallback ci ay xët yu ñu tànn.

**Aksé** — xool xët yi ñu gis (`worker/src/render/`) ci sartu WCAG; CSP bi dafa dëgër (amul `unsafe-inline`), kon solusiyon yi ci biir loolu lañu war a dox.

**Ergonomi API** — `worker/src/routes/` — pajinasyon, filtré, deskripsyon OpenAPI, kliyan yu ay misaal.

**Kàttanu pipeline** — ay yoon yu gën a wàññiku, gën a xamal avancement, cas yu mujj ci gistar delta (`pipeline/lib/delta.py`).

**Mbindi** — `docs/20260511/` (繁體中文; `00-*` moy index bi). Tekki ci angale mbindi xarala yi am na solo.

### Sartu cosaan

- Yoon yépp dañuy relativ — proje bi dafa war a mën a dox ci masin yépp. Bulleen dugal yoon absolut yu dëgër.
- Buleen yokk benn dependans bu pip ci ab modil cëru pipeline. Etap yu opte yi mën nañoo jëfandikoo ay pake yu opte, te dañuy war a wàññiku ci ni mu waree su nekkul.
- Buleen wàññi dooley nosteg jëm-kanam rekk — loolu moy àtte bu njëg bi.
- Buleen dugal mandarga bu ofisel bu ngornamaŋ U.S., te buleen yokk dara luy dindi li ñu nëbb.
- Soppi ci schema D1 dafay laal **ñaari** fisiye: `pipeline/lib/manifest_schema.sql` ak `db/schema.sql`.
- Ay test ak kod bu bees. Messaasi commit yu Conventional Commits.

Liirleen `CLAUDE.md` ak `docs/20260511/00-*` bu njëkk, ubbileen benn jafe-jafe ngir waxtaan ci dara lu jëm ci nosteg bi laata ngeen di def PR.

