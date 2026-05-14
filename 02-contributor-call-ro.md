# GitHub — Postarea 2 din 3 · Apel pentru contribuitori / „good first issues”

**Utilizare:** o Discuție fixată („Contribuții & good first issues”) sau o introducere pentru CONTRIBUTING.md.
**Cuvinte cheie:** open source, contribuții, good first issue, i18n, localizare, OCR, Python, TypeScript, Vitest, pytest, accesibilitate, UAP, date deschise
**Hyperlinkuri:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cum să contribuiți la ufolens.com

[ufolens.com](https://www.ufolens.com) transformă [arhiva PURSUE UAP](https://www.war.gov/ufo) a Departamentului de Război al S.U.A. într-o platformă multilingvă, căutabilă, cu un [API public](https://www.ufolens.com/api/v1). Este compus din două jumătăți — un pipeline local de ingestie în Python (`pipeline/`) și o aplicație edge în TypeScript/Hono (`worker/`) — care se întâlnesc într-o singură interfață: un pachet publicat de SQL + resurse.

Nu aveți nevoie de credențiale de cloud pentru a contribui. Modulele de bază ale pipeline-ului folosesc doar biblioteci standard, iar testele pentru Worker rulează folosind stocare în memorie.

### Configurare

```bash
# pipeline
python3 -m pytest pipeline/tests/          # totul ar trebui să fie verde, nu necesită instalare cu pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Unde este ajutorul cel mai util

**i18n / localizare** — `worker/src/i18n/ui-strings.json` este sursa șirurilor de text din interfața de utilizare. O revizuire de către un vorbitor nativ a oricărei localizări non-engleze este de mare valoare: identificarea traducerilor automate nepotrivite, corectarea problemelor de RTL/layout, îmbunătățirea cazurilor speciale de negociere a limbii.

**Calitatea OCR** — pre-procesare mai bună a scanărilor vechi, dactilografiate, înainte de OCR; un sistem de evaluare care compară motorul open-source cu fallback-ul Tesseract pe pagini de exemplu.

**Accesibilitate** — auditarea paginilor randate (`worker/src/render/`) conform WCAG; CSP-ul este strict (fără `unsafe-inline`), deci soluțiile trebuie să funcționeze în acest cadru.

**Ergonomia API-ului** — `worker/src/routes/` — paginare, filtrare, descriere OpenAPI, clienți de exemplu.

**Robustețea pipeline-ului** — mai multe căi de degradare graduală, raportare mai bună a progresului, cazuri speciale pentru detecția de diferențe (`pipeline/lib/delta.py`).

**Documentație** — `docs/20260511/` (繁體中文; `00-*` este indexul). Traducerile documentelor de design în engleză sunt binevenite.

### Reguli de bază

- Toate căile sunt relative — proiectul trebuie să fie portabil pe diferite mașini. Fără căi absolute hardcodate.
- Nu adăugați dependențe pip la un modul *de bază* al pipeline-ului. Etapele opționale pot folosi pachete opționale și trebuie să se degradeze gradual fără ele.
- Nu slăbiți mașina de stări forward-only — aceasta reprezintă plafonul de cost.
- Nu introduceți însemne oficiale ale guvernului S.U.A. și nu adăugați nimic care să anuleze cenzurările din sursă.
- Modificările de schemă D1 afectează **două** fișiere: `pipeline/lib/manifest_schema.sql` și `db/schema.sql`.
- Cod nou, însoțit de teste. Mesaje de commit în format Conventional Commits.

Citiți mai întâi `CLAUDE.md` și `docs/20260511/00-*`, apoi deschideți un issue pentru a discuta orice modificare structurală înainte de a crea un PR.

