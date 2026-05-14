# GitHub — Bäitrag 2 vun 3 · Bäitrags-Opruff / "good first issues"

**Benotzen als:** eng ugepinnte Diskussioun ("Bäidroen & good first issues") oder eng CONTRIBUTING.md Aféierung.
**Schlësselwierder:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bäidroen zu ufolens.com

[ufolens.com](https://www.ufolens.com) mécht aus dem U.S. War Department sengem [PURSUE UAP-Archiv](https://www.war.gov/ufo) eng duerchsichbar, méisproocheg Plattform mat enger [ëffentlecher API](https://www.ufolens.com/api/v1). Et sinn zwou Halschenten — eng lokal Python Ingest-Pipeline (`pipeline/`) an eng TypeScript/Hono Edge-App (`worker/`) — déi sech an enger Interface treffen: e publizéierte Bündel aus SQL + Assets.

Dir braucht keng Cloud-Umeldungsinformatioune fir bäizedroen. D'Kär-Module vun der Pipeline sinn stdlib-only an d'Worker-Tester lafen géint en In-Memory-Späicher.

### Installatioun

```bash
# Pipeline
python3 -m pytest pipeline/tests/          # sollt alles gréng sinn, keng pip-Installatioun néideg

# Worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Wou Hëllef am nëtzlechsten ass

**i18n / Lokaliséierung** — `worker/src/i18n/ui-strings.json` ass d'Quell vun den UI-Strings. D'Iwwerpréiwung vun all net-englescher Sprooch duerch e Mammesproochler ass vu groussem Wäert: komesch maschinell Iwwersetzunge fannen, RTL-/Layout-Problemer fixéieren, d'Grenzfäll vun der Sprooch-Verhandlung verbesseren.

**OCR-Qualitéit** — besser Virveraarbechtung vun alen, mat der Schreifmaschinn geschriwwene Scans virun der OCR; en Evaluatiounskader, deen d'Open-Source-Engine mam Tesseract-Fallback op Beispillsäite vergläicht.

**Accessibilitéit** — en Audit vun de gerenderte Säiten (`worker/src/render/`) géint d'WCAG; d'CSP ass streng (keng `unsafe-inline`), also mussen d'Léisungen an deem Kader funktionéieren.

**API-Ergonomie** — `worker/src/routes/` — Paginatioun, Filteren, OpenAPI-Beschreiwung, Beispill-Clienten.

**Pipeline-Stabilitéit** — méi Weeër fir e kontrolléierten Ausfall, besser Fortschrëtts-Berichterstattung, Grenzfäll bei der Delta-Erkennung (`pipeline/lib/delta.py`).

**Doku** — `docs/20260511/` (繁體中文; `00-*` ass den Index). Iwwersetzunge vun den Design-Dokumenter op Englesch si wëllkomm.

### Grondreegelen

- All Pfadë relativ — de Projet muss iwwer verschidde Maschinnen portabel sinn. Keng hardcodéiert absolut Pfadë.
- Füügt keng pip-Ofhängegkeet zu engem Pipeline-Kärmodul bäi. Optional Etappen däerfen optional Päck benotzen, a mussen ouni si kontrolléiert ausfalen.
- Schwächt d'Forward-only State Machine net — dat ass d'Käschte-Plafong.
- Füügt keng offiziell US-Regierungs-Insignie bäi, a füügt näischt bäi, wat d'Redaktioune vun der Quell réckgängeg mécht.
- D1 Schema-Ännerungen betreffen **zwee** Fichieren: `pipeline/lib/manifest_schema.sql` an `db/schema.sql`.
- Tester mat neiem Code. Conventional-commit-Messagen.

Liest `CLAUDE.md` an `docs/20260511/00-*` als éischt, da maacht en Issue op, fir alles Strukturellt ze diskutéieren, ier Dir e PR maacht.
