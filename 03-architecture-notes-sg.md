# GitHub — Kiri 3 ti 3 · Atënë na ndo ti architecture (Tokua ti ADR)

**Tongana ti sara kua na ni:** mbeni Tokua na gbe ti "Fa na hinga" / "Architecture", wala mbeni semence ti ADR ti `docs/`.
**Atënë ti nda ni:** architecture, ADR, machine ti état so ayeke gue gi na li ni, LLM ti ndo ni, Ollama, OCR, edge computing, CSP, a-header ti sécurité, pipeline ti atënë, ingénierie ti futango ye, manifeste ti SQLite, D1, R2, KV
**Aroko:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Nda ni so a leke ufolens.com na fason so

Atënë na ndo ti a-décision ota so asara si [ufolens.com](https://www.ufolens.com) (reconstruction so a lingbi ti gi yâ ni na ayanga ti kodro mingi ti [archive ti PURSUE UAP](https://www.war.gov/ufo)) ayeke na fini. A yeke wara a-commentaire / a-pushback.

### 1. Pipeline ni ayeke mbeni machine ti état so ayeke gue gi na li ni — na lege ni

A-état: `a wara → a download → ocr_done → a traduire → a fa na gigi`. Mbeni document ayeke gue gi na li ni, na gi tongana a yeke na kua ti sarango ni. A yeke kiri ti sara kua na atënë so a fa na gigi lâ oko pëpe gi tongana mbeni détecteur ti delta abâ so nda ni a changé biani.

**Nda ni:** OCR + traduction ayeke akusala so ayeke ngere, na archive ni ayeke kono na lege ti ngoi. Mbeni pipeline so "ayeke kiri ti sara ye kue ti yeke na sécurité" ayeke na mbeni futango so ayeke na limite pëpe. Sarango si a-transition na peko adescend pëpe ayeke sara si mbeni facture so ayeke kpe ayeke na place pëpe. Plafond ti futango ye ayeke mbeni ye ti graphe ti état, pëpe ti beku ti zo ti kua.

**Futango ni:** a-migration ti schéma na kirikiri sarango kua na ndo ni ayeke sioni na lege ni. A yeke mbeni compromis so a lingbi ti yeda na ni.

### 2. OCR na traduction ayeke tambela na ndo ti mbeni LLM ti ndo ni, pëpe mbeni API ti cloud

OCR: moteur open-source, fallback ti Tesseract CLI. Traduction + NER: Gemma na lege ti Ollama, na ndo ti mbeni laptop ti Apple Silicon.

**Nda ni:** zero futango ti marginal ndali ti mbeni document oko; a lingbi ti kiri ti sara ni (modèle so a fixer + a-prompt); na étape ti gingo ye ni doit ti tambela awe na lege ti mbeni IP ti da (nda ni ayeke na peko ti Akamai Bot Manager — `curl` ayeke wara mbeni 403), tongaso mbeni laptop ayeke na yâ ti boucle ni awe.

**Futango ni:** nzoni ti traduction ni ayeke na gbe ti mbeni modèle ti frontière. Ndali ti mbeni corpus ti référence so Anglais ti nda ni ayeke gi na mbeni clic oko, so ayeke nzoni. E tene pëpe so a-traduction ni ayeke na lege ni.

### 3. A-partie use ni ayeke na mbeni interface oko so ayeke gi: mbeni paquet so a fa na gigi

Pipeline ni ayeke sû lâ oko pëpe na yâ ti base de données ti production. A yeke fa na gigi `{ SQL, manifeste ti ye so a yeke na ni, liste ti purger cache }`. "Fango na gigi" = sara kua na paquet ni na li ni (pousse SQL na edge SQL DB, sara synchronisation ti a-asset na stockage ti objet, purger a-clé ti cache so a iri).

**Nda ni:** mbage ti ndo ni na mbage ti edge a lingbi ti kono na lege ti ala mveni; a lingbi ti bâ paquet ni; na "déployer atënë" ayeke na forme oko na ngoi kue. Worker ni ayeke mbeni application ti TypeScript/Hono so ayeke kete — CSP so ayeke ngangu (`unsafe-inline` pëpe; inline JSON-LD ayeke sha256-pinned), `Accept-Language` + négociation ti kodro→yanga ti kodro, cache ti page ti KV ti lango 30, cron ti kusala ti da ti lango oko oko — na a yeke na bezoin lâ oko pëpe ti hinga tongana nyen a sara atënë ni.

**Futango ni:** mbeni changement ti schéma ti D1 ayeke ndu a-fichier use (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Mbeni assurance so ayeke ngere pëpe.

### A-ye so a lingbi ti sara tënë na ndo ni pëpe so a zîa na yâ ti comportement

- A yeke ti gouvernement ti États-Unis pëpe; mbeni insigne officiel pëpe.
- A yeke bata a-redaction ti nda ni, a yeke kiri na peko ni lâ oko pëpe.
- A mû vidéo na DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` na ndo ti site kue — a lingbi ti indexé na lege ti gingo ye, a yeke opt-out ti AI-scrape.

A yeke na fini: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
