# GitHub — Færsla 2 af 3 · Kallað eftir framlögum / „góð fyrstu verkefni“

**Nota sem:** Festar umræður („Framlög og góð fyrstu verkefni“) eða inngangur að CONTRIBUTING.md.
**Lykilorð:** opinn hugbúnaður, framlög, gott fyrsta verkefni, i18n, staðfærsla, OCR, Python, TypeScript, Vitest, pytest, aðgengi, UAP, opin gögn
**Tenglar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Að leggja sitt af mörkum til ufolens.com

[ufolens.com](https://www.ufolens.com) breytir [PURSUE UAP skjalasafni](https://www.war.gov/ufo) bandaríska varnarmálaráðuneytisins í leitanlegan, fjöltyngdan vettvang með [opnu API](https://www.ufolens.com/api/v1). Þetta eru tveir helmingar — staðbundin Python inntakspípulína (`pipeline/`) og TypeScript/Hono brúnforrit (`worker/`) — sem mætast í einu viðmóti: útgefnum SQL + eignapakka.

Þú þarft engin skýjaauðkenni til að leggja þitt af mörkum. Kjarnaeiningar pípunnar eru aðeins með stdlib-ósjálfstæði og prófanir fyrir Worker keyra gegn minnisgeymslu.

### Uppsetning

```bash
# pipeline
python3 -m pytest pipeline/tests/          # ætti allt að vera grænt, engin pip install þörf

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Hvar hjálp nýtist best

**i18n / staðfærsla** — `worker/src/i18n/ui-strings.json` er uppspretta texta fyrir notendaviðmótið. Yfirlestur móðurmálshafa á hvaða staðfærslu sem er, annarri en enskri, er mikils virði: laga klaufalegar vélþýðingar, lagfæra RTL/útlitsvandamál, bæta jaðartilvik í tungumálastjórnun.

**Gæði OCR** — betri forvinnsla á gömlum vélrituðum skönnunum fyrir OCR; matsumgjörð sem ber saman opna hugbúnaðinn við Tesseract til vara á sýnishornssíðum.

**Aðgengi** — úttekt á útfærðum síðum (`worker/src/render/`) gegn WCAG; CSP er strangt (ekkert `unsafe-inline`), þannig að lausnir verða að virka innan þeirra marka.

**Vistvænni API** — `worker/src/routes/` — síðufletting, síun, OpenAPI lýsing, dæmi um biðlara.

**Stöðugleiki pípunnar** — fleiri leiðir til að virka með minni afköstum, betri framvinduskýrslur, jaðartilvik í delta-skynjun (`pipeline/lib/delta.py`).

**Skjölun** — `docs/20260511/` (繁體中文; `00-*` er efnisyfirlitið). Þýðingar á hönnunarskjölunum yfir á ensku eru vel þegnar.

### Grunnreglur

- Allar slóðir hlutfallslegar — verkefnið verður að vera færanlegt milli véla. Engar fastkóðaðar algerar slóðir.
- Ekki bæta við pip-ósjálfstæði í *kjarna*einingu pípunnar. Valkvæð stig mega nota valkvæða pakka, og verða að virka með minni afköstum án þeirra.
- Ekki veikja áfram-aðeins stöðuvélina — hún er kostnaðarþakið.
- Ekki setja inn opinber merki bandarískra stjórnvalda, og ekki bæta við neinu sem afmáir yfirstrikanir í frumskjölum.
- D1 skemabreytingar snerta **tvær** skrár: `pipeline/lib/manifest_schema.sql` og `db/schema.sql`.
- Próf með nýjum kóða. Conventional-commit skilaboð.

Lestu `CLAUDE.md` og `docs/20260511/00-*` fyrst, og opnaðu svo mál til að ræða um grundvallarbreytingar áður en þú sendir inn PR.
