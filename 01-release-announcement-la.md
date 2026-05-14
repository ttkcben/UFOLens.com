# GitHub — Nuntius I ex III · Nuntius editionis / caput pro README

**Uti ut:** corpus editionis GitHub, disputatio fixa, aut caput repositorii README.
**Claves verborum:** UAP, UFO, archivum PURSUE, documenta secreta revelata, data aperta, quaestio textus pleni, OCR, translatio machinalis, LLM localis, Ollama, computatio in margine, API publicum, Hono, TypeScript, Python
**Hypertextus:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — suggestus multilinguis et pervestigabilis pro archivo PURSUE UAP

**In vivo:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Archivum fontis:** https://www.war.gov/ufo

`ufolens.com` re-publicat archivum **PURSUE** a Departmento Belli Civitatum Foederatarum de monumentis UAP / UFO secretis revelatis ut suggestum scientiae: quaestio textus pleni, translatio machinalis per totum corpus, exploratio per tabulas geographicas et temporales, et API publicum JSON. Documenta fontis sunt opera gubernationis foederalis Civitatum Foederatarum et intra Civitates Foederatas in dominio publico sunt ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Hoc consilium **non est affiliatum gubernationi Civitatum Foederatarum**, nullis insignibus officialibus utitur, et numquam redactiones invertit.

### Architectura

```
Machina localis (Apple Silicon, IP residentialis)    Rete in margine
──────────────────────────────────────────         ─────────────────────────
pipeline/ (Python 3.10, nucleus stdlib-tantum)       worker/ (TypeScript, Hono.js)
  capta → OCR → translata → publica (solum progrediens) /{lang}/...   paginae
  OCR: machina fontis aperti (Tesseract CLI subsidiaria) /api/v1/...   API publicum
  translatio / NER: LLM localis (Gemma per Ollama)      /admin        consola operatoris
  status: manifestum SQLite                          sustentatur a: DB SQL in margine,
        │                                              repositorio obiectorum (PDFs fontis),
        └── publicat fasciculum: SQL + manifestum bonorum + indicem purgationis cache ──┘
```

- **Nullus sumptus per documentum in nube AI.** OCR et translatio localiter currunt; machina statuum quae solum progreditur (`inventum → receptum → ocr_perfectum → translatum → publicatum`) efficit ne quod documentum denuo tractetur, nisi mutatum sit.
- **Nucleus catenae operum nullas dependentias a tertiis partibus habet** — moduli ad parsing / manifestum / differentias currunt et probantur in Python puro sine ullis `pip install`; stadia OCR/translationis gratiose degradantur cum fasciculi optionales absunt.
- **Situs in margine** applicat strictos capitis securitatis + CSP (nullum `unsafe-inline`; JSON-LD in linea per sha256-fixum), negotiationem linguae per `Accept-Language` + cartographiam nationis, cache paginarum in KV per 30 dies, et cron quotidianum ad sustentationem.
- **Additiones incrementales:** detector differentiarum indicem fontis comparat et solum mutationes in catenam operum rursus alit.

### Ad programmatores

API publicum apud https://www.ufolens.com/api/v1 documenta et metadatam ut JSON reddit. Accessus anonymus rate-limitatus est; pete clavem pro gradibus inquisitoris/programmatatoris. Vide sectionem API in situ pro punctis terminalibus et limitibus.

### Status

Codex completus; situs apud https://www.ufolens.com explicatus est. Basis datorum productionis impletur currendo catenam operum extra lineam et fasciculum progrediendo publicando (`cli_publish run --remote`). Documenta designationis plena in `docs/20260511/` habitant.

### Licentia / limites

- Documenta fontis: Opera gubernationis foederalis Civitatum Foederatarum, in dominio publico intra Civitates Foederatas.
- Codex proprius huius suggestus: vide `LICENSE`.
- Situs mittit `Tdm-Reservation: 1` et `X-Robots-Tag: noai, noimageai` — indicabilis a machinis quaestionis, optatus ex exercitatione/exscriptione AI.
- Materialia video attributa sunt DVIDS / AARO et non ab hoc consilio vindicantur.

Problemata et PRs grata sunt. Lege, quaeso, `CLAUDE.md` et `docs/20260511/00-*` antequam mutationes structurales aperias.

