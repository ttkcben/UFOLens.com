# GitHub — Post 1 of 3 · Gbɔŋgɔrtoo / README gbɔŋgɔrtɔɔ nɛɛlɛŋ

**Tɔzɩ kpee nɛ:** GitHub Gbɔŋgɔrtɔɔ kɛlɛkɛlɛ, ŋgbannɩ Tɔm, yaa repo README yɔɔ lɩmaɣza.
**Yaaŋ hɔɔlɩŋ:** UAP, UFO, PURSUE archive, takayaɣ sɔɔlɩm, tɔm yɔɔŋ pɩlɩnɩ hɛhɛɛ, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Lanaa taa kpaŋŋa:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP archive taa tɔm yɔɔŋ piilii nɛ ɛ-nɔɔ nɛ ɛ-kɛlɛkɛlɛ nɛ ɛ-ŋgbannɩŋ

**Paalɩsɩ:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Takayaɣ taa tɔm yɔɔŋ:** https://www.war.gov/ufo

`ufolens.com` ɖosuu U.S. War Department ɛ-**PURSUE** archive taa UAP / UFO takayaɣ sɔɔlɩm tɔm nɛ kɛlɛkɛlɛ. Kɛ kpeŋŋa nɛ tɔm yɔɔŋ pɩlɩnɩ ɛ-taa nɛ full-text search, machine translation nɛ ɛ-nɔɔ kpeekpe, map + timeline kpɛlɛŋ, nɛ JSON API yɔɔŋ tɔzɩŋ. Takayaɣ tɔm yɔɔŋ lɛ U.S. federal government ɛ-kɔɔ nɛ U.S. taa lɛ, ɛ-kpɛŋa nɛ ɛ-tɔm yɔɔŋ tɔzɩŋ. Kɛ public domain ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Gbɔŋgɔrtoo kpaŋŋa kpaɣ **U.S. government ɛ-taa tɔm**. Tɔm yɔɔŋ sɔɔlɩm, insignia, nɛ Redactions taa tɔm pɩlɩnɩ ɛ-taa.

### Architecture

```
Local machine (Apple Silicon, residential IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Cloud-AI siŋŋ ɛ-kpɛŋa**. OCR nɛ translation lɛ, ɛ-wɩlɩŋ. Pipeline taa tɔm yɔɔŋ pɩlɩnɩ ɛ-taa nɛ forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) ɛ-takayaɣ tɔm yɔɔŋ gbɔŋgɔrtoo tɩkpɛŋŋŋŋ.
- **Pipeline taa tɔm yɔɔŋ** lɛ, ɛ-kpɛŋa nɛ third-party dependencies. Python stdlib-only core nɛ Python pip-installed modules. OCR/translation stages lɛ, ɛ-tɔm yɔɔŋ gbɔŋgɔrtoo.
- **Edge site** lɛ, ɛ-security headers + CSP (no `unsafe-inline`; inline JSON-LD sha256-pinned), language negotiation via `Accept-Language` + country mapping, 30-day KV page cache, nɛ daily housekeeping cron.
- **Incremental updates:** delta detector lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo.

### Developers ɛ-tɔm yɔɔŋ

Public API https://www.ufolens.com/api/v1 lɛ, ɛ-documents nɛ metadata nɛ JSON tɔm yɔɔŋ. Anonymous access lɛ, rate-limited. Researcher/developer tiers lɛ, ɛ-key tɔm. API section lɛ, ɛ-endpoints nɛ limits.

### Nɛɛlɛŋ

Code gbɔŋgɔrtoo; site https://www.ufolens.com taa. Production database lɛ, ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo. Pipeline nɛ `cli_publish run --remote`. Full design docs `docs/20260511/` taa.

### License / boundaries

- Source documents: U.S. federal government works, public domain within the U.S.
- This platform's own code: `LICENSE` taa tɔm.
- Site lɛ, ɛ-`Tdm-Reservation: 1` nɛ `X-Robots-Tag: noai, noimageai` tɔm yɔɔŋ. Search engines ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo.
- Video footage lɛ, DVIDS / AARO ɛ-ufolens.com archive taa tɔm yɔɔŋ gbɔŋgɔrtoo.

Issues nɛ PRs kɛlɛkɛlɛ. `CLAUDE.md` nɛ `docs/20260511/00-*` gbɔŋgɔrtoo tɩkpɛŋŋŋŋ.

