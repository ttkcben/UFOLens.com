# GitHub — Dieđáhus 2/3 · Veahkideaddjibovdehus / "buorit vuosttaš áššit"

**Geavat dego:** giddanuvvon ságastallamin ("Veahkideapmi & buorit vuosttaš áššit") dahje CONTRIBUTING.md álggahussan.
**Čoavddasánit:** rabas gáldu, veahkideapmi, buorre vuosttaš ášši, i18n, báikáiduhttin, OCR, Python, TypeScript, Vitest, pytest, ollesahttivuohta, UAP, rabas dáhta
**Hyperliŋkkat:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Veahkideapmi ufolens.com:i

[ufolens.com](https://www.ufolens.com) rievdada U.S. Soahtedepartemeantta [PURSUE UAP-arkiivva](https://www.war.gov/ufo) ohcanlágan, máŋggagielat vuođđun mas lea [almmolaš API](https://www.ufolens.com/api/v1). Dás leat guokte oasi — báikkálaš Python sisafievrridan-pipeline (`pipeline/`) ja TypeScript/Hono edge-prográmma (`worker/`) — mat deaivvadit ovtta gaskaboddasis: almmuhuvvon SQL + aktiva-buđaldus.

Ii dárbbaš balvadohkendieđuid veahkaváldimii. Pipeline váimmusmodulat leat dušše stdlib-sorjavacca ja Worker-testtat doibmet muittobáikkálaš vuorkká vuostá.

### Sajáiduhttin

```bash
# pipeline
python3 -m pytest pipeline/tests/          # galggašii leat buot ruoná, ii dárbbaš pip-installerema

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Gos veahkki lea ávkkáleamos

**i18n / báikáiduhttin** — `worker/src/i18n/ui-strings.json` lea UI-teavsttaid gáldu. Eatnigielat olbmo dárkun mii beare ii-eaŋgalasgielat báikáduhttimis lea hui árvvolaš: gávdnat váivves mašinabuktosa, divvut RTL/layout-váttisvuođaid, buoridit giellanegošierema earenoamášdiliid.

**OCR-kvalitehta** — buoret ovdagieđahallan boares, čállinmašiinain čállojuvvon skánnemiin ovdal OCR; árvvoštallanhárjehusa buohtastahttin mii veardida open-source-mohtora Tesseract-várenuppožuhttima vuostá omdasiidduin.

**Ollesahttivuohta** — dárkkis buvttaduvvon siidduid (`worker/src/render/`) WCAG vuostá; CSP lea čavga (ii `unsafe-inline`), nu ahte čovdosat fertejit doibmat dan siste.

**API-ergonomiija** — `worker/srcsrc/routes/` — siidojuohkin, silleheapmi, OpenAPI-čilgehus, omdaklienttat.

**Pipeline nanusvuohta** — eanet čábbát njiedjan bálgát, buoret ovdáneami raportteren, delta-fuobmána earenoamášdilit (`pipeline/lib/delta.py`).

**Dokumeanttat** — `docs/20260511/` (繁體中文; `00-*` lea indeaksa). Plánaáššegirjjiid jorgaleamit eaŋgalasgillii leat buresboahtin.

### Vuođđonjuolggadusat

- Buot bálgát relatiivvalaččat — prošeakta ferte leat sirdinlágan mašiinnaid gaskkas. Ii guoske absoluhtalaš bálgáid.
- Ii lasit pip-sorjavašvuođa pipeline *váimmus*modulai. Optionála dásit sáhttet geavahit optionála páhkkaid, ja fertejit čábbát njiedjat daid haga.
- Ii hehtet ovddosguvlui-dušše stáhtamašiidna — dat lea gollogeahči.
- Ii lasit virggálaš U.S. ráđđehusa dovdomearkkaid, iige lasit maidege mii guorole gáldosensureringiid.
- D1 skemmarievdadusat gusket **guovtti** fiilai: `pipeline/lib/manifest_schema.sql` ja `db/schema.sql`.
- Testtat ođđa kodain. Conventional-commit-dieđáhusat.

Loga `CLAUDE.md` ja `docs/20260511/00-*` vuos, ja de álgge ášši ságastallat juoga struktuvrralaččas ovdalgo PR.
