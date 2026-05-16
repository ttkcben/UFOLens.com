# GitHub — Gi Fino'na na Post 1 de 3 · Anunsio Fina'nu'i / Anunsio README block

**Usa komo:** un GitHub Release na kuerpo, un pinned Discussion, pat i sanhilo' na README gi repo.
**Primas na Palåbra:** UAP, UFO, PURSUE archive, dinisklasifika na dokumentos, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — un platforma multi-lengguåhi, manhånao para i PURSUE UAP archive

**Lålå'la':** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Original na archive:** https://www.war.gov/ufo

`ufolens.com` fuma'nu'i ta'lo i U.S. War Department's **PURSUE** archive ni dinisklasifika na UAP / UFO na rekots komo un knowledge platforma: full-text search, machine translation gi todu i corpus, mapa + timeline na eksplorasion, yan un public JSON API. I original na dokumentos manche'cho' i gubetnamenton U.S. federal yan public domain giya U.S. ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Este na proyekto **ti offisial na afiliåo gi gubetnamenton U.S.**, taya' offisial na insinia, yan taya' na mumenli'e' redactions.

### Fine'chong

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

- **Taya' per-dokumenton cloud-AI gastos.** I OCR yan translation manmamampos giya lokal; i forward-only state machine (`discovered → downloaded → ocr_done → translated → published`) kumonfotma na taya' dokumento ni manreprusecha ta'lo lokkue' yanggen esta mamoddå'i.
- **I pipeline core taya' third-party dependencies** — i parsing / manifest / delta modules manmampos yan manprubao gi un clean Python ni taya' pip-installed; i OCR/translation stages manmamoddå'i malålu' yanggen i optional packages taya'.
- **Edge site** gumagåggao strict security headers + CSP (no `unsafe-inline`; inline JSON-LD sha256-pinned), language negotiation via `Accept-Language` + country mapping, un 30-day KV page cache, yan un daily housekeeping cron.
- **Incremental updates:** un delta detector mumenli'e' i source index yan ha na'chalek i changes gi pipeline.

### Para i developers

I public API gi https://www.ufolens.com/api/v1 ha na'i dokumentos yan metadata komo JSON. I anonymous access limitado gi rate; gaogaohao un key para i researcher/developer tiers. Li'e' i API na patte gi website para i endpoints yan limits.

### Estao

Mata'chå'an i kodigo; i website maninaplika gi https://www.ufolens.com. I production database maninina'gaga'on ni i running i offline pipeline yan i publishing i bundle forward (`cli_publish run --remote`). I todu na design docs manmamampos gi `docs/20260511/`.

### Lisensia / tåotåo

- Original na dokumentos: Che'cho' i gubetnamenton U.S. federal, public domain within the U.S.
- Este na platforma na kodigo: li'e' `LICENSE`.
- I site ha send `Tdm-Reservation: 1` yan `X-Robots-Tag: noai, noimageai` — indexable ni search engines, opted out of AI training/scraping.
- Video footage maninina'attribuida gi DVIDS / AARO yan ti manclaimed ni este na proyekto.

I Issues yan PRs manwelkåm. Fannguålo' `CLAUDE.md` yan `docs/20260511/00-*` antes de i opening structural changes.

