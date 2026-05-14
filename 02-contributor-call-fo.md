# GitHub — Støða 2 av 3 · Kunning til hjálparfólk / "góð fyrstu mál"

**Nýt sum:** ein festur Tjaktráður ("Soleiðis hjálpir tú til & góð fyrstu mál") ella ein inngangur í CONTRIBUTING.md.
**Leitorð:** open source, hjálpa til, gott fyrsta mál, i18n, staðseting, OCR, Python, TypeScript, Vitest, pytest, atgongd, UAP, opin data
**Hyperleinkjur:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Soleiðis kanst tú hjálpa til við ufolens.com

[ufolens.com](https://www.ufolens.com) ger [PURSUE UAP-skjalasavnið](https://www.war.gov/ufo) hjá U.S. War Department til ein leitanligan, fleirmálsligan pall við einum [almennum API](https://www.ufolens.com/api/v1). Tað eru tvær helvtir — ein lokal Python-inntøkurørskipan (`pipeline/`) og ein TypeScript/Hono edge-app (`worker/`) — sum møtast á einum markamóti: einum útgivnum SQL + assets-bundli.

Tú tørvar ongar ský-innritunarupplýsingar fyri at hjálpa til. Kjarnumodulirnir í rørskipanini eru einans stdlib, og royndirnar hjá Worker koyra móti minnisgoymslu.

### Uppseting

```bash
# pipeline
python3 -m pytest pipeline/tests/          # alt skuldi verið grønt, ongin pip-install er neyðugur

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Hvar hjálp er mest gagnlig

**i18n / staðseting** — `worker/src/i18n/ui-strings.json` er keldan til UI-streingir. Gjøgnumgongd av móðurmælstalum av øllum ikki-enskum máløkjum er sera virðismikil: finn ósmædna maskinútgávu, rætta RTL/útlitsfeilir, betra um mál-samráðingar-undantøk.

**OCR-góðska** — betri forviðgerð av gomlum, skrivmaskinuskrivaðum skanningum áðrenn OCR; ein metingarramma, sum samanber open-source-motorin við Tesseract-varaleiðina á royndarsíðum.

**Atgongd** — endurskoða rendu síðurnar (`worker/src/render/`) mótvegis WCAG; CSP er strongur (einki `unsafe-inline`), so loysnir mugu virka innanfyri tað.

**API-ergonomi** — `worker/src/routes/` — síðuskifting, filtrering, OpenAPI-lýsing, dømi um klientar.

**Styrki í rørskipanini** — fleiri leiðir til hóvligan niðurgang, betri frágreiðing um framgongd, undantøk í delta-uppdaging (`pipeline/lib/delta.py`).

**Skjøl** — `docs/20260511/` (繁體中文; `00-*` er indeksið). Týðingar av sniðgevingarskjølunum til enskt eru vælkomnar.

### Grundleggjandi reglur

- Allar slóðir eru relatívar — verkætlanin skal kunna flytast millum maskinur. Ongar harðkodaðar absoluttar slóðir.
- Legg onga pip-binding til ein *kjarna*-modul í rørskipanini. Valfrí stig kunnu nýta valfríar pakkar, og mugu hóvliga niðurbróta uttan teir.
- Veik ikki um einans-fram eftir gongdandi støðumaskina — tað er kostnaðarloftið.
- Legg ikki almenn amerikansk stjórnarmerki til, og legg ikki nakað til, sum vendir keldu-strikingum um.
- D1-skemabroytingar nema við **tvær** fílur: `pipeline/lib/manifest_schema.sql` og `db/schema.sql`.
- Royndir við nýggjari kodu. Conventional-commit-fráboðanir.

Les `CLAUDE.md` og `docs/20260511/00-*` fyrst, og opna síðan eitt issue fyri at umrøða nakað strukturelt áðrenn PR.
