# GitHub — Chigawo 2 pa 3 · Kuyitana kwa othandizira / "nkhani zabwino zoyambira"

**Gwiritsani ntchito ngati:** Zokambirana zoyikidwa ("Kuthandizira & nkhani zabwino zoyambira") kapena mawu oyamba a CONTRIBUTING.md.
**Mawu ofunikira:** open source, kuthandizira, nkhani yabwino yoyambira, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Ma hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kuthandizira ku ufolens.com

[ufolens.com](https://www.ufolens.com) imasandutsa [PURSUE UAP archive](https://www.war.gov/ufo) ya Unduna wa Nkhondo waku U.S. kukhala nsanja yosakika, yotanthauzira malilime ambiri yokhala ndi [API yoyera](https://www.ufolens.com/api/v1). Ili ndi magawo awiri — pipi ya Python yakumaloko (`pipeline/`) ndi pulogalamu ya TypeScript/Hono edge (`worker/`) — akukumana pa mawonekedwe amodzi: bundle yosindikizidwa ya SQL + zida.

Simufunika zizindikiro zilizonse za cloud kuti muthandizire. Magawo akuluakulu a pipi ndi stdlib-only ndipo mayeso a Worker amayendetsa motsutsana ndi malo osungira mu-memory.

### Kukhazikitsa

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kumene thandizo ndilothandiza kwambiri

**i18n / localization** — `worker/src/i18n/ui-strings.json` ndiye gwero la ma string a UI. Kuwunika kwa chilankhulo china chilichonse osati Chingerezi ndikofunikira kwambiri: pezani zolakwika za makina, konzani mavuto a RTL/layout, sinthani zolakwika za kukambirana chilankhulo.

**Ubwino wa OCR** — kukonzekera bwino kwa zojambula zakale zolembedwa ndi makina musanachitike OCR; chida choyesera chofananizira injini ya open-source motsutsana ndi Tesseract fallback pa masamba angapo.

**Accessibility** — funsani masamba operekedwa (`worker/src/render/`) motsutsana ndi WCAG; CSP ndi yokhwima (palibe `unsafe-inline`), kotero mayankho ayenera kugwira ntchito mkati mwake.

**API ergonomics** — `worker/src/routes/` — pagination, filtering, OpenAPI description, zitsanzo za makasitomala.

**Kukhoza kwa Pipeline** — njira zambiri zochepetsera mavuto, malipoti abwino a patsogolo, delta-detection edge cases (`pipeline/lib/delta.py`).

**Zolemba** — `docs/20260511/` (繁體中文; `00-*` ndi index). Kumasulira zolemba zamapangidwe ku Chingerezi ndikolandiridwa.

### Malamulo oyambira

- Njira zonse zofanana — pulojekitiyi iyenera kusamutsidwa pakati pa makina. Palibe njira zovuta zowongoka.
- Musawonjezere kudalira kwa pip ku gawo lalikulu la pipi. Magawo osankhika angagwiritse ntchito mapaketi osankhika, ndipo ayenera kuchepetsa mavuto mosavuta popanda iwo.
- Musachepetse makina a state machine omwe amangopita patsogolo — ndiye malire a mtengo.
- Musawonjezere zizindikiro zovomerezeka za boma la U.S., ndipo musawonjezere chilichonse chomwe chimabwezeretsa zosasinthika zoyambira.
- Zosintha za D1 schema zimakhudza mafayilo **awiri**: `pipeline/lib/manifest_schema.sql` ndi `db/schema.sql`.
- Mayeso ndi code yatsopano. Mauthenga a Conventional-commit.

Werengani `CLAUDE.md` ndi `docs/20260511/00-*` poyamba, kenako tsegulani nkhani kuti mukambirane chilichonse chokhudza mapangidwe musanachitike PR.

