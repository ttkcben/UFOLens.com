# GitHub — Ingingo 2 kuri 3 · Guhamagarira abafatanyabikorwa / "ibibazo byiza byo gutangiriraho"

**Rikoreshwe nka:** Ikiganiro gihamye ("Gutanga umusanzu & ibibazo byiza byo gutangiriraho") cyangwa intangiriro ya CONTRIBUTING.md.
**Amagambo y'ingenzi:** isoko rifunguye, gutanga umusanzu, ikibazo cyiza cyo gutangiriraho, i18n, guhindura mu rurimi rw'aho, OCR, Python, TypeScript, Vitest, pytest, kugera ku bantu bose, UAP, amakuru afunguye
**Imiyoboro:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Gutanga umusanzu kuri ufolens.com

[ufolens.com](https://www.ufolens.com) ihindura [ububiko bwa PURSUE UAP](https://www.war.gov/ufo) bw'Ishami ry'Intambara rya Leta Zunze Ubumwe z’Amerika mo urubuga rushobora gushakishwamo, rw'indimi nyinshi rufite [API rusange](https://www.ufolens.com/api/v1). Rigizwe n'ibice bibiri — umuyoboro w'imbere mu gihugu wa Python (`pipeline/`) na porogaramu y'impera ya TypeScript/Hono (`worker/`) — bihura ku murongo umwe: umuzingo wa SQL + umutungo wasohowe.

Ntabwo ukeneye ibyangombwa byo mu gicu kugira ngo utange umusanzu. Module z'ibanze z'umuyoboro zikoresha stdlib gusa kandi ibizamini bya Worker bikorerwa mu bubiko bwo mu mutwe.

### Gutegura

```bash
# pipeline
python3 -m pytest pipeline/tests/          # byose bigomba kuba byiza, nta pip install ikenewe

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Aho ubufasha bukenewe cyane

**i18n / guhindura mu rurimi rw'aho** — `worker/src/i18n/ui-strings.json` niyo soko y'amagambo yo ku rubuga. Igenzura ry'umuntu uvuga ururimi kavukire ku rurimi rutari Icyongereza rifite agaciro kanini: gukosora imvugo idasobanutse y'imashini, gukosora ibibazo bya RTL/imiterere, kunoza uburyo bwo guhitamo ururimi.

**Ubwiza bwa OCR** — gutegura neza inyandiko za kera zandikishijwe imashini mbere ya OCR; uburyo bwo kugereranya moteri ya open-source n'uburyo bwisumbuye bwa Tesseract ku mpapuro z'icyitegererezo.

**Kugera ku bantu bose** — kugenzura impapuro zasohowe (`worker/src/render/`) ugereranyije na WCAG; CSP irakomeye (nta `unsafe-inline`), bityo ibisubizo bigomba gukora muri ubwo buryo.

**Uburyo bworoshye bwa API** — `worker/src/routes/` — gupagika, kuyungurura, ibisobanuro bya OpenAPI, ingero z'abakiriya.

**Gukomera k'umuyoboro** — uburyo bwinshi bwo gupfa buhoro buhoro, gutanga raporo nziza y'iterambere, ibibazo by'inyongera mu gutahura delta (`pipeline/lib/delta.py`).

**Inyandiko** — `docs/20260511/` (繁體中文; `00-*` ni urutonde). Ubusemuzi bw'inyandiko z'imiterere mu Cyongereza bwakiranwa yombi.

### Amategeko shingiro

- Inzira zose zigomba kuba zishingiye ku aho ziri — umushinga ugomba kuba ushobora kwimurwa ku mashini zose. Nta nzira zihamye zanditswemo.
- Ntukongeremo ishingiro rya pip kuri module *y'ibanze* y'umuyoboro. Ibyiciro by'inyongera bishobora gukoresha porogaramu z'inyongera, kandi bigomba gupfa buhoro buhoro iyo zidahari.
- Ntukoroshye imashini y'imiterere igana imbere gusa — nicyo giciro ntarengwa.
- Ntukongeremo ibirango bya leta ya Leta Zunze Ubumwe z’Amerika, kandi ntukongeremo ikintu na kimwe gihindura ibyahishwe mu nkomoko.
- Impinduka z'imiterere ya D1 zikora ku fayili **ebyiri**: `pipeline/lib/manifest_schema.sql` na `db/schema.sql`.
- Ibizamini hamwe na kode nshya. Ubutumwa bwa Conventional-commit.

Soma `CLAUDE.md` na `docs/20260511/00-*` mbere, hanyuma utangize ikibazo kugira ngo muganire ku kintu cyose cy'imiterere mbere ya PR.

