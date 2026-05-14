# GitHub — Poostâ 2/3 · Išedeijee-koččom / "pyereh vuostâš iskeh"

**Kevttim:** kiŋŋituttum Savâstâlmân ("Išedem & pyereh vuostâš iskeh") teikâ CONTRIBUTING.md aalgân.
**Suárkisanah:** ávus koodi, išedem, pyeri vuostâš iske, i18n, lokalistem, OCR, Python, TypeScript, Vitest, pytest, almostâttem, UAP, ávus data
**Hyperliŋkah:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Išedem ufolens.com-proojeekt

[ufolens.com](https://www.ufolens.com) nubásmit Ovtâstum väldikodij Soâtidepartment [PURSUE UAP -arkkâdâh](https://www.war.gov/ufo) uuccâmnáál, maaŋgâkielâlâš vuáđun, mast lii [almolâš API](https://www.ufolens.com/api/v1). Tot lii kyehti pelni — páihálâš Python-vuolgâttemputkâ (`pipeline/`) já TypeScript/Hono-roovdiiräiji-app (`worker/`) — moh teivâdeh oovtâ interfeisist: almostittum SQL + oomeet-pookkijd.

Ij taarbâš mige pilvâ-tubdâlduvâid išediđ. Putkâvuáđu-moduleh láá stdlib-tuš já Worker-testih joteh muštolâšvuárkká vuástá.

### Vuáđudem

```bash
# pipeline
python3 -m pytest pipeline/tests/          # kalgaššii leđe puoh ruánááh, ij taarbâš pip-installeerim

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kost iše lii eenâmus ávkálâš

**i18n / lokalistem** — `worker/src/i18n/ui-strings.json` lii UI-merkkâráiđui algâalgâttâh. Eidin kielâ sárnoo árvuštâllâm mihtuin ij-eŋgâlâskielâ lokaalist lii styeres árvu: kavnâ hirmâd mašin-jurgâlusâid, tivvo RTL/asâttâs-čulmáid, pyetee kielâ-šopâmuš roovdiiräiji-tilijd.

**OCR-kvaliteet** — pyereb ovdâkeevâttem puáris kipto-kirjeem skannimijn ovdil OCR; árvuštâllâm-vuáđđudem, mii väärid ávus koodi moottor já Tesseract-varamoottor valdum siijđoin.

**Almostâttem** — tärhist almostittum siijđoid (`worker/src/render/`) WCAG vuástá; CSP lii čuolmâd (ij `unsafe-inline`), nuuvt et čuávduseh kalgeh toimâđ ton siste.

**API-ergonomia** — `worker/src/routes/` — siijđo-juohhum, sillem, OpenAPI-kuvâldâh, ovdâmerkkâ-klientih.

**Putkâ-kestasvuotâ** — eenâb árbulâš-nuvâdem-pálgáh, pyereb ovdánem-raportistem, delta-tievdâttem roovdiiräiji-tileh (`pipeline/lib/delta.py`).

**Dokumenteh** — `docs/20260511/` (繁體中文; `00-*` lii indeeks). Vuáváámdokumentij jurgâlusah eŋgâlâskielân láá tiervâpuáttim.

### Vuáđu-njuolgâdusah

- Puoh pálgáh relatiivliih — proojeekt kalga leđe sirdeemnáál mašinij kooskâ. Igeen čuolmâd absoluttisii pálgáid.
- Älä lasse pip-kulâlâšvuođâ putkâ *vuáđu*-modulân. Vällimnallâš mudoheh pyehtih kevttiđ vällimnallâš paakeetijd, já kalgeh árbulávt nuuvâđ toi illá.
- Älä hiäjusmit ovdâskulkee tilá-mašin — tot lii koloi alarääji.
- Älä lasse almâlâš Ovtâstum väldikodij haldâttuvvâ tubdâlduvâid, iäge lasse mige, mii jurâs algâalgâttuv čapisvuotâid.
- D1-schema-nubástusah kyeskih **kyehti** tiätturááid: `pipeline/lib/manifest_schema.sql` já `db/schema.sql`.
- Testih uđđâ koodáin. Conventional-commit-saavah.

Luuvâ `CLAUDE.md` já `docs/20260511/00-*` vuossin, já rábá vihâlisto savâstâllâđ mihtuin ruttâdâh-ääšist ovdil PR.

