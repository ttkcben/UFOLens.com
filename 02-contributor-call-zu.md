# GitHub — Okuthunyelwe 2 koku-3 · Ubizo lwabanikeli / "izinkinga zokuqala ezinhle"

**Sebenzisa njenge:** i-Discussion ebhiniwe ("Ukunikela nezinkinga zokuqala ezinhle") noma isingeniso se-CONTRIBUTING.md.
**Amagama ayisihluthulelo:** umthombo ovulekile, ukunikela, inkinga yokuqala enhle, i-i18n, ukwenziwa kwasendaweni, i-OCR, i-Python, i-TypeScript, i-Vitest, i-pytest, ukufinyeleleka, i-UAP, idatha evulekile
**Ama-hyperlink:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Ukunikela ku-ufolens.com

I-[ufolens.com](https://www.ufolens.com) iguqula [ingobo yomlando ye-PURSUE UAP](https://www.war.gov/ufo) yoMnyango Wezempi wase-U.S. ibe inkundla eseshekayo, enezilimi eziningi ene-[API yomphakathi](https://www.ufolens.com/api/v1). Kuyizingxenye ezimbili — i-pipeline yokungenisa ye-Python yendawo (`pipeline/`) nohlelo lokusebenza lwe-TypeScript/Hono edge (`worker/`) — ezihlangana kusixhumi esisodwa: isixha esishicilelwe se-SQL + sezimpahla.

Awudingi ziqinisekiso zamafu ukuze unikele. Amamojuli angumongo we-pipeline yi-stdlib-kuphela futhi izivivinyo ze-Worker zisebenza ngokumelene nesitoreji esisemenoryini.

### Ukusetha

```bash
# pipeline
python3 -m pytest pipeline/tests/          # kufanele konke kube luhlaza, akukho ukufakwa kwe-pip okudingekayo

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Lapho usizo luwusizo kakhulu

**i18n / ukwenziwa kwasendaweni** — i-`worker/src/i18n/ui-strings.json` ingumthombo wemisho ye-UI. Ukubuyekezwa kwesikhulumi sendabuko kwanoma iyiphi indawo engeyona yesiNgisi kunenani eliphakeme: thola umphumela womshini ongajwayelekile, lungisa izinkinga ze-RTL/zesakhiwo, thuthukisa amacala anqenqemeni okuxoxisana ngolimi.

**Ikhwalithi ye-OCR** — ukucutshungulwa kwangaphambili okungcono kwama-scan amadala athayiphiwe ngaphambi kwe-OCR; i-harness yokuhlola eqhathanisa injini yomthombo ovulekile ne-Tesseract fallback emakhasini ayisampula.

**Ukufinyeleleka** — hlola amakhasi anikeziwe (`worker/src/render/`) ngokumelene ne-WCAG; i-CSP iqinile (akukho `unsafe-inline`), ngakho-ke izixazululo kufanele zisebenze ngaphakathi kwalokho.

**I-ergonomics ye-API** — `worker/src/routes/` — ukuhlukanisa ngamakhasi, ukuhlunga, incazelo ye-OpenAPI, amaklayenti ayisibonelo.

**Ukuqina kwe-pipeline** — izindlela eziningi zokwehla kahle, ukubika inqubekela phambili okungcono, amacala anqenqemeni okuthola i-delta (`pipeline/lib/delta.py`).

**Amadokhumenti** — `docs/20260511/` (繁體中文; i-`00-*` iyinkomba). Ukuhumusha kwamadokhumenti okuklama kuya esiNgisini kwamukelekile.

### Imithetho eyisisekelo

- Zonke izindlela zihlobene — iphrojekthi kufanele ikwazi ukuthwalwa emishinini ehlukene. Azikho izindlela eziphelele ezifakwe ngokuqinile.
- Ungangezi ukuncika kwe-pip kumojuli *womongo* we-pipeline. Izigaba ozikhethela zingasebenzisa amaphakheji ozikhethela, futhi kufanele zehle kahle ngaphandle kwawo.
- Ungawenzi buthaka umshini wesimo oya phambili kuphela — lowo umkhawulo wezindleko.
- Ungangenisi izimpawu ezisemthethweni zikahulumeni wase-U.S., futhi ungangezi lutho oluhlehlisa ukuhlelwa komthombo.
- Izinguquko zesikimu se-D1 zithinta amafayela **amabili**: i-`pipeline/lib/manifest_schema.sql` ne-`db/schema.sql`.
- Izivivinyo ngekhodi entsha. Imilayezo ye-conventional-commit.

Funda i-`CLAUde.md` ne-`docs/20260511/00-*` kuqala, bese uvula inkinga ukuze nixoxe nganoma yini yesakhiwo ngaphambi kwe-PR.

