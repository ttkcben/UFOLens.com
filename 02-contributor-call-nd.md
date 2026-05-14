# GitHub — Post 2 of 3 · Umnikelo wabaxhasi / "izinkinga ezinhle zokuqala"

**Sebenzisa njenge:** i-Discussion efakiwe ("Ukufaka isandla & izinkinga ezinhle zokuqala") kumbe isingeniso se-CONTRIBUTING.md.
**Amagama angukhiye:** i-open source, ukufaka isandla, inkinga yokuqala enhle, i18n, ukwenza kube okwasendaweni, OCR, Python, TypeScript, Vitest, pytest, ukufinyeleleka, UAP, idatha evulekile
**Izixhumanisi:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ukufaka isandla ku-ufolens.com

[ufolens.com](https://www.ufolens.com) iguqula i-U.S. War Department's [PURSUE UAP archive](https://www.war.gov/ufo) ibe inkundla eseshekayo, enezilimi eziningi ene-[public API](https://www.ufolens.com/api/v1). Yingxenye ezimbili — i-local Python ingest pipeline (`pipeline/`) kanye ne-TypeScript/Hono edge app (`worker/`) — zihlangana endaweni eyodwa: i-SQL eshicilelwe + i-assets bundle.

Awudingi noma yiziphi izifakazelo ze-cloud ukuze unikele. Amamojula angukhiye e-pipeline yi-stdlib-only kanti ukuhlolwa kwe-Worker kwenziwa ngokuphikisana ne-in-memory storage.

### Ukusetha

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Lapho usizo luwusizo kakhulu

**i18n / ukwenza kube okwasendaweni** — `worker/src/i18n/ui-strings.json` ungumthombo wezintambo ze-UI. Ukubuyekezwa kwesikhulumi sendabuko kwanoma yiluphi ulwimi olungesiNgisi kubaluleke kakhulu: ukubamba okuphumayo komshini okungahleliwe, ukulungisa izinkinga ze-RTL/layout, ukuthuthukisa izimo ze-language-negotiation edge.

**Ikhwalithi ye-OCR** — ukucutshungulwa okungcono kokuskenwa okudala okuthayiphwe ngaphambi kwe-OCR; ukuhlola i-harness eqhathanisa injini ye-open-source ne-Tesseract fallback kumapheji esampula.

**Ukufinyeleleka** — hlola amapheji akhiqiziwe (`worker/src/render/`) ngokuqhathanisa ne-WCAG; i-CSP iqinile (akukho `unsafe-inline`), ngakho izixazululo kumele zisebenze ngaphakathi kwalokho.

**I-API ergonomics** — `worker/src/routes/` — ukuhlukanisa amakhasi, ukuhlunga, incazelo ye-OpenAPI, izibonelo zamaklayenti.

**Ukuqina kwe-Pipeline** — izindlela zokwehliswa kancane okungcono, ukubika inqubekelaphambili okungcono, izimo ze-delta-detection edge (`pipeline/lib/delta.py`).

**Amadokhumende** — `docs/20260511/` (繁體中文; `00-*` inkomba). Ukuhunyushwa kwamalungiselelo okuklama esiNgisini kwamukelekile.

### Imithetho eyisisekelo

- Zonke izindlela zihlobene — iphrojekthi kumele ithwale phakathi kwemishini. Akukho zindlela eziqinile ezibhalwe kanzima.
- Ungafaki ukuncika kwe-pip ku-pipeline *core* module. Izigaba ezingakhethwa zingasebenzisa amaphakheji angakhethwa, futhi kumele zehle ngokuhle kakhulu ngaphandle kwawo.
- Ungawuqedisi umshini we-state oya phambili kuphela — yizinga lezindleko eziphezulu.
- Ungafaki uphawu olusemthethweni lukahulumeni wase-U.S., futhi ungafaki noma yini ehlehlisa ukuhlelwa komthombo.
- Izinguquko zeskimu se-D1 zithinta amafayela **amabili**: `pipeline/lib/manifest_schema.sql` kanye `db/schema.sql`.
- Ukuhlolwa ngekhodi entsha. Imilayezo ye-Conventional-commit.

Funda `CLAUDE.md` kanye `docs/20260511/00-*` kuqala, bese uvula inkinga ukuze nixoxe noma yini ehlobene nesakhiwo ngaphambi kwe-PR.

