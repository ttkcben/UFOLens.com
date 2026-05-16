# GitHub — Afichaj 1 sou 3 · Anons Piblikasyon / Blòk Anons README

**Itilize kòm:** yon kò GitHub Release, yon Diskisyon epengle, oswa tèt README depo a.
**Mo kle:** UAP, UFO, achiv PURSUE, dokiman deklasifye, done ouvè, rechèch tèks konplè, OCR, tradiksyon machin, LLM lokal, Ollama, edge computing, API piblik, Hono, TypeScript, Python
**Lyen ipètèks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — yon platfòm miltiling, kote ou ka fè rechèch, pou achiv PURSUE UAP

**Live:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Achiv sous:** https://www.war.gov/ufo

`ufolens.com` re-pibliye achiv **PURSUE** Depatman Lagè Etazini an sou dosye UAP / UFO deklasifye yo kòm yon platfòm konesans: rechèch tèks konplè, tradiksyon machin atravè tout kòpus la, eksplorasyon kat + delè, ak yon API JSON piblik. Dokiman sous yo se travay gouvènman federal Etazini e Ozetazini yo nan domèn piblik ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Pwojè sa a **pa afilye ak gouvènman Etazini**, li pa itilize okenn anblèm ofisyèl, e li pa janm retire redaksyon yo.

### Achitekti

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

- **Zewo pri cloud-AI pou chak dokiman.** OCR ak tradiksyon kouri lokalman; machin eta a ki sèlman deplase devan (`discovered → downloaded → ocr_done → translated → published`) garanti okenn dokiman pa trete ankò sof si li chanje.
- **Nwayo pipeline la pa gen depandans twazyèm pati** — modil analiz / manifès / delta kouri epi teste sou yon Python pwòp san okenn pip enstale; etap OCR/tradiksyon yo degrade grasyeuz lè pakè opsyonèl yo absan.
- **Sit Edge** aplike antèt sekirite strik + CSP (pa gen `unsafe-inline`; JSON-LD inline sha256-epengle), negosyasyon langaj atravè `Accept-Language` + kat peyi, yon kach paj KV ki dire 30 jou, ak yon cron antretyen chak jou.
- **Mizajou inkremantal:** yon detektè delta fè diferans ant endèks sous la epi sèlman retounen chanjman yo nan pipeline la.

### Pou devlopè

API piblik la nan https://www.ufolens.com/api/v1 retounen dokiman ak metadata kòm JSON. Aksè anonim limite nan pousantaj; mande yon kle pou nivo chèchè/devlopè. Gade seksyon API sou sit la pou pwen fen ak limit.

### Estati

Kòd konplè; sit deplwaye nan https://www.ufolens.com. Baz done pwodiksyon an popile lè yo kouri pipeline offline a epi pibliye pakèt la devan (`cli_publish run --remote`). Dokiman konsepsyon konplè yo nan `docs/20260511/`.

### Lisans / limit

- Dokiman sous: travay gouvènman federal Etazini, domèn piblik Ozetazini.
- Kòd platfòm sa a: gade `LICENSE`.
- Sit la voye `Tdm-Reservation: 1` ak `X-Robots-Tag: noai, noimageai` — motè rechèch ka endekse li, men li pa patisipe nan fòmasyon/ekstraksyon AI.
- Videyo yo atribiye a DVIDS / AARO e pwojè sa a pa reklame yo.

Pwotokòl ak PRs akeyi. Tanpri li `CLAUDE.md` ak `docs/20260511/00-*` anvan ou louvri chanjman estriktirèl.

