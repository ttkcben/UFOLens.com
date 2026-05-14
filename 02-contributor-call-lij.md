# GitHub — Post 2 de 3 · Ciàmmo a-i contributoî / "prìmmi travàggi bon"

**Dêuvo:** cómme 'na Discusción fisâ ("Contribuçioìn e prìmmi travàggi bon") ò 'n'introduçión a CONTRIBUTING.md.
**Paròlle ciave:** open source, contriboî, prìmmo travàggio bon, i18n, localizaçión, OCR, Python, TypeScript, Vitest, pytest, acesibilitæ, UAP, dæti avèrti
**Colegamenti ipertestoâli:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuì a ufolens.com

[ufolens.com](https://www.ufolens.com) o trasfórma l'[archivio PURSUE UAP](https://www.war.gov/ufo) do Dipartimento da Goæra di Stati Unîi 'nt'na piattaforma riçercàbile e multilingua con 'n'[API pùblica](https://www.ufolens.com/api/v1). O l'é fæto de doê meitæ — 'n pipeline de ingèstión locâle in Python (`pipeline/`) e 'n'app a-o bòrdo in TypeScript/Hono (`worker/`) — che s'incóntran in sce 'n'unica interfàccia: 'n pachetto publicòu de SQL + assets.

No gh'avéi de bezéugno de credensiâli cloud pe contribuì. I mòdoli centrâli do pipeline són solo-stdlib e i test do Worker gîan cóntra 'n'archiviaçión in memöia.

### Preparaçión

```bash
# pipeline
python3 -m pytest pipeline/tests/          # dovieivan êse tutti vèrdi, no serve instalâ nìnte con pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Dond'è che l'agiùtto o l'é ciù ùtile

**i18n / localizaçión** — `worker/src/i18n/ui-strings.json` o l'é a vivàgna de strìnghe de l'interfàccia utente. A revixón de 'n parlànte natîvo de unn-a quàlunque localitæ no ingléize a l'é de gràn valô: trovâ e traduçioìn aotomàtiche sgrêuzie, coreze i problemi de RTL/layout, megioâ i câxi lìmite da negociaçión da léngoa.

**Qualitæ de l'OCR** — megioâ a pre-elaboraçión de vêge scanscioìn scrite a màchina prìmma de l'OCR; 'n scistêma de valutaçión pe conparâ o motô open-source co-o fallback de Tesseract in sce de pàgine de ezénpio.

**Acesibilitæ** — controlâ e pàgine renderizæ (`worker/src/render/`) cóntra i stàndard WCAG; o CSP o l'é strétto (nisciùn `unsafe-inline`), dónca e soluçioìn dêvan fonçionâ rispetàndolo.

**Ergonomîa de l'API** — `worker/src/routes/` — paginaçión, filtràggio, descriçión OpenAPI, client de ezénpio.

**Robustéssa do Pipeline** — ciù percórsci de degradaçión grasiôza, megioâ a segnalaçión do progrèsso, câxi lìmite da rilevaçión de delta (`pipeline/lib/delta.py`).

**Documentaçión** — `docs/20260511/` (繁體中文; `00-*` o l'é l'ìndice). E traduçioìn di documenti de progetaçión in ingléize són benvegnûe.

### Régole de bâze

- Tutti i percórsci són relatîvi — o progètto o dêve êse portàbile tra e màchine. Nisciùn percórso asolûto codificòu.
- No azonze 'na dipendénsa pip a 'n mòdolo *centrâle* do pipeline. E fâze òpçionâli pêuan adêuviâ pachetti òpçionâli, e dêvan degradâ con gràçia sénsa de quélli.
- No indebolî a màchina a stâti solo in avanti — quélla a l'é a garançîa do còsto màscimo.
- No introdûe insìgne ofiçiâli do govèrno di Stati Unîi, e no azonze nìnte che revèrse e redaçioìn da vivàgna.
- I cangiaménti a-o schêma D1 tócan **doî** file: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- I test con còdice nêuvo. Mesàggi de commit convençionâli.

Lezéi prìmma `CLAUDE.md` e `docs/20260511/00-*`, pöi avrî 'n issue pe discùtte qualónque cangiaménto struturâle prìmma da PR.

