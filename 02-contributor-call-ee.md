# GitHub — Nyatakaka 2 le 3 me · Kpekpeɖeŋu-nana yɔyɔ / "dɔwɔna gbãtɔ nyuiwo"

**Zã abe:** Nyamedzro si woƒo ka ("Kpekpeɖeŋu Nana & dɔwɔna gbãtɔ nyuiwo") alo CONTRIBUTING.md ƒe ŋgɔdonya.
**Nya veviwo:** open source, kpekpeɖeŋu nana, dɔwɔna gbãtɔ nyui, i18n, gbe-me-dɔwɔwɔ, OCR, Python, TypeScript, Vitest, pytest, amesiame ƒe zazateŋgɔ, UAP, open data
**Kadodo veviwo:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kpekpeɖeŋu Nana ufolens.com

[ufolens.com](https://www.ufolens.com) trɔa U.S. Aʋawɔnyawo Gbɔkpɔƒe ƒe [PURSUE UAP archive](https://www.war.gov/ufo) wò-zua nu-ɖoanyi si me wo-teaŋu di-na nu le gbe-ge-ɖewo me kple [public API](https://www.ufolens.com/api/v1). Akpa eve mee wole — Python pipeline si nɔa teƒe si wɔa dɔ (`pipeline/`) kple TypeScript/Hono edge app (`worker/`) — wodoa go le nu ɖeka me: SQL + assets ƒe bundle si wo-ta.

M-a-hia alilikpo me kpekpeɖeŋu aɖeke hafi na-teŋu a-kpe ɖe eŋu o. Pipeline ƒe dɔwɔnu vevitɔwo nye stdlib-ɖeɖe ko eye Worker ƒe dodokpɔwo zɔna ɖe nu-dzraɖoƒe si le susu me ŋu.

### Ðoɖowɔwɔ

```bash
# pipeline
python3 -m pytest pipeline/tests/          # elebe woa-nye amamu blibo, pip install aɖeke me-hiã o

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Afisiwo kpekpeɖeŋu hiã le wu

**i18n / gbe-me-dɔwɔwɔ** — `worker/src/i18n/ui-strings.json` ye nye UI nyawo ƒe gɔmedzenu. Ame-si do gbe la ƒe numekɔkplɔ̃ le gbe-si me-nye Eŋlisigbe o me la xɔa asi sãsãsã: kpɔ mɔ̃ɖegɔmeɖeɖe siwo me-sɔ o, ɖɔ RTL/ɖoɖo kuxíwo ɖo, eye na-ɖɔ gbe-domeɖeɖe ƒe nɔnɔme tɔxɛwo ɖo.

**OCR ƒe nyinyemenɔnɔme** — dɔwɔwɔ doŋgɔ nyuie le agbalẽ xoxowo dzi hafi wo-wɔ OCR; dodo-kpɔ nu-ɖoanyi si sɔa open-source mɔ̃a kple Tesseract ƒe kpeɖeŋua sɔna le aŋgba ɖewo dzi.

**Amesiame ƒe zazateŋgɔ** — wɔ dɔ le aŋgba siwo wo-ɖe fia dzi (`worker/src/render/`) ɖe WCAG ŋu; CSP la sesẽ (aɖeke mele `unsafe-inline` me o), eyata elebe mɔfiamewo na-wɔ dɔ le seɖoƒe ma me.

**API zazã bɔbɔe** — `worker/src/routes/` — aŋgba-mama, nu-dzadzraɖo, OpenAPI ƒe gɔmeɖeɖe, mɔ̃ zazã ƒe kpɔɖeŋuwo.

**Pipeline ƒe dɔsesẽnɔnɔme** — dɔwɔwɔ bɔbɔe ƒe mɔ geɖewo, dɔwɔwɔ ƒe ŋgɔ-yiyi ŋuti nya nyuiewo, delta-didi ƒe nɔnɔme tɔxɛwo (`pipeline/lib/delta.py`).

**Agbalẽwo** — `docs/20260511/` (繁體中文; `00-*` ye nye index la). Mí-kpɔa dzidzɔ ɖe gɔmeɖeɖe tso design agbalẽawo me yi Eŋlisigbe me ŋu.

### Kɔpi-sɛwo

- Mɔwo katã li — elebe dɔwɔna la na-teŋu a-wɔ dɔ le mɔ̃ vovovowo dzi. Mɔ siwo ƒe teƒe trɔna la mele eme o.
- Me-gatsɔ nu-si hiã pip ɖe pipeline *core* module me o. Dɔwɔna tɔxɛwo teaŋu zãa nu tɔxɛwo, eye elebe woa-wɔ dɔ nyuie ne esiawo me-le eme o hã.
- Me-ga-ɖe ŋgɔ-yiyi-ɖeɖe ƒe nɔnɔme mɔ̃ la ƒe ŋusẽ dzi kpɔtɔ o — eyae nye ga-zazã ƒe seɖoƒea.
- Me-ga-tsɔ U.S. dziɖuɖu ƒe dzesi aɖeke kpe ɖe eŋu o, eye me-ga-tsɔ naneke si a-trɔ nu-siwo wo-ɖe le gɔmedzenu la me o.
- D1 schema ƒe tɔtrɔwo kaa faɛl **eve** nu: `pipeline/lib/manifest_schema.sql` kple `db/schema.sql`.
- Dodokpɔwo kple code yeyewo. Conventional-commit gbe-ɖeɖewo.

Xlẽ `CLAUDE.md` kple `docs/20260511/00-*` gbã, emegbe na-ʋu kuxí aɖe na-dzro nu gã aɖe ƒomevi me hafi na-wɔ PR.

