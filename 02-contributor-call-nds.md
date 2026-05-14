# GitHub — Bidrag 2 vun 3 · Oproop för Bidreger / "gode eerste Issues"

**Bruken as:** en fastpinnte Diskusschoon ("Bidragen & gode eerste Issues") oder en INFÖHREN för CONTRIBUTING.md.
**Slötelwöör:** Open Source, Bidragen, good eerste Issue, i18n, Lokalisatschoon, OCR, Python, TypeScript, Vitest, pytest, Togänglichkeit, UAP, apen Daten
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Bidragen to ufolens.com

[ufolens.com](https://www.ufolens.com) maakt ut dat [PURSUE UAP-Archiv](https://www.war.gov/ufo) vun’t U.S. War Department en dörsökbore, mehrsprakige Plattform mit en [apentliche API](https://www.ufolens.com/api/v1). Dat sünd twee Halvdelen — en lokale Python-Ingest-Pipeline (`pipeline/`) un en TypeScript/Hono Edge-App (`worker/`) — de sik an een Schnittsteed draapt: en publizeert SQL + Assets-Bünnel.

Ji bruukt keen Cloud-Togangsdaten, üm bitodragen. De Karnmodulen vun de Pipeline sünd blots stdlib un de Worker-Tests loopt gegen en In-Memory-Spieker.

### Inrichten

```bash
# pipeline
python3 -m pytest pipeline/tests/          # schull all gröön ween, keen pip-Installatschoon nödig

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Wo Hülp an’n nützlichsten is

**i18n / Lokalisatschoon** — `worker/src/i18n/ui-strings.json` is de Born för UI-Strings. En Överprüfen dör Moderspraaklers vun jede nich-engelsche Lokaal is vun hogen Weert: unbeholpen maschinelle Utgaven finnen, RTL-/Layout-Problemen beheben, Randfäll bi de Spraakverhanneln verbetern.

**OCR-Qualität** — betere Vörverarbeiden vun ole, mit de Maschien schrevene Scans vör OCR; en Evaluatschoons-Harness, de de Open-Source-Engine mit den Tesseract-Fallback op Bispeelsieden vergliekt.

**Togänglichkeit** — överprüüft de renderten Sieden (`worker/src/render/`) gegen WCAG; de CSP is strikt (keen `unsafe-inline`), dorüm mööt Lösen binnen dit Rahmenwark funkschoneren.

**API-Ergonomie** — `worker/src/routes/` — Paginatschoon, Filtern, OpenAPI-Beschrieven, Bispeel-Clients.

**Pipeline-Robustheit** — mehr graziöse Degradatschoonsweeg, betere Fortschrittsberichten, Randfäll bi de Delta-Detektschoon (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` is de Index). Översetten vun de Design-Dokmenten na Engelsch sünd willkamen.

### Grundregeln

- All Paaie relativ — dat Projekt mutt över Maschinen portabel ween. Keen hardkodeerte absolute Paaie.
- Föögt keen pip-Afhängigkeit to en Pipeline-*Karn*-Modul to. Optionale Stopen köönt optionale Paketen bruken un mööt ahn jem graziöös degradeern.
- Swäckt de blots-vörwärts-Tostandsmaschien nich af — dat is de Kosten-Bövergrenz.
- Föögt keen offiziellen Insignien vun de U.S.-Regeren to, un föögt nix to, wat Born-Redaktschonen trüggängig maakt.
- D1-Schema-Ännern raakt **twee** Datein: `pipeline/lib/manifest_schema.sql` un `db/schema.sql`.
- Tests mit ne’en Kood. Narichten in’n Stil vun Conventional Commits.

Leest `CLAUDE.md` un `docs/20260511/00-*` toeerst, denn maakt en Issue apen, üm wat Strukturells to besnacken, vördem Ji en PR maakt.

