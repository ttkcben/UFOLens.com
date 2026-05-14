# GitHub — Postimi 2 nga 3 · Thirrje për kontribues / "çështje të mira për fillestarë"

**Përdorimi si:** një Diskutim i fiksuar ("Kontributi & çështje të mira për fillestarë") ose një hyrje në CONTRIBUTING.md.
**Fjalë kyçe:** burim i hapur, kontribut, çështje e mirë për fillestarë, i18n, lokalizim, OCR, Python, TypeScript, Vitest, pytest, aksesueshmëri, UAP, të dhëna të hapura
**Hiperlinqe:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kontributi në ufolens.com

[ufolens.com](https://www.ufolens.com) e shndërron [arkivin PURSUE UAP](https://www.war.gov/ufo) të Departamentit të Luftës së SHBA-së në një platformë të kërkueshme, shumëgjuhëshe me një [API publike](https://www.ufolens.com/api/v1). Ai përbëhet nga dy gjysma — një pipeline lokal Python për marrjen e të dhënave (`pipeline/`) dhe një aplikacion edge TypeScript/Hono (`worker/`) — që takohen në një ndërfaqe: një pako e publikuar SQL + asete.

Nuk ju nevojiten kredenciale cloud për të kontribuar. Modulet kryesore të pipeline-it janë vetëm me stdlib dhe testet e Worker-it ekzekutohen kundrejt ruajtjes në memorie.

### Konfigurimi

```bash
# pipeline
python3 -m pytest pipeline/tests/          # duhet të jenë të gjitha jeshile, nuk nevojitet instalim me pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Ku ndihma është më e dobishme

**i18n / lokalizim** — `worker/src/i18n/ui-strings.json` është burimi i teksteve të ndërfaqes së përdoruesit. Rishikimi nga folës amtarë i çdo gjuhe jo-angleze është me vlerë të lartë: kapni rezultate të çuditshme makinerike, rregulloni çështjet RTL/paraqitjes, përmirësoni rastet e veçanta të negocimit të gjuhës.

**Cilësia e OCR** — para-përpunim më i mirë i skanimeve të vjetra të shtypura me makinë shkrimi përpara OCR; mjet vlerësimi që krahason motorin me burim të hapur me rezervën Tesseract në faqe shembull.

**Aksesueshmëria** — auditoni faqet e renderuara (`worker/src/render/`) kundrejt WCAG; CSP është i rreptë (pa `unsafe-inline`), kështu që zgjidhjet duhet të funksionojnë brenda këtij kufizimi.

**Ergonomia e API-së** — `worker/src/routes/` — paginimi, filtrimi, përshkrimi OpenAPI, klientë shembull.

**Qëndrueshmëria e Pipeline-it** — më shumë rrugë degradimi të hijshëm, raportim më i mirë i progresit, raste të veçanta të zbulimit të delta-s (`pipeline/lib/delta.py`).

**Dokumentacioni** — `docs/20260511/` (繁體中文; `00-*` është indeksi). Përkthimet e dokumenteve të dizajnit në anglisht janë të mirëpritura.

### Rregullat bazë

- Të gjitha shtigjet relative — projekti duhet të jetë i transportueshëm ndërmjet makinave. Asnjë shteg absolut i koduar fort.
- Mos shtoni një varësi pip në një modul *kryesor* të pipeline-it. Fazat opsionale mund të përdorin paketa opsionale dhe duhet të degradojnë me hijeshi pa to.
- Mos e dobësoni makinën e gjendjes vetëm-përpara — kjo është tavani i kostos.
- Mos prezantoni emblema zyrtare të qeverisë së SHBA-së dhe mos shtoni asgjë që kthen mbrapsht redaktimet burimore.
- Ndryshimet e skemës D1 prekin **dy** skedarë: `pipeline/lib/manifest_schema.sql` dhe `db/schema.sql`.
- Teste me kodin e ri. Mesazhe konvencionale të commit-it.

Lexoni `CLAUDE.md` dhe `docs/20260511/00-*` fillimisht, pastaj hapni një çështje për të diskutuar çdo gjë strukturore përpara PR-së.

