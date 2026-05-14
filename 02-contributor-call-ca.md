# GitHub — Publicació 2 de 3 · Crida a col·laboradors / "bons primers issues"

**Ús:** com a discussió fixada ("Contribucions i bons primers issues") o una introducció a `CONTRIBUTING.md`.
**Paraules clau:** codi obert, contribuir, bon primer issue, i18n, localització, OCR, Python, TypeScript, Vitest, pytest, accessibilitat, UAP, dades obertes
**Enllaços:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Com contribuir a ufolens.com

[ufolens.com](https://www.ufolens.com) converteix l'[arxiu PURSUE UAP](https://www.war.gov/ufo) del Departament de Guerra dels EUA en una plataforma cercable i multilingüe amb una [API pública](https://www.ufolens.com/api/v1). Consta de dues meitats —un pipeline d'ingestió local en Python (`pipeline/`) i una aplicació edge en TypeScript/Hono (`worker/`)— que es troben en una única interfície: un paquet publicat de SQL + actius.

No necessiteu cap credencial del núvol per contribuir. Els mòduls principals del pipeline només depenen de la llibreria estàndard i les proves del Worker s'executen contra un emmagatzematge en memòria.

### Configuració

```bash
# pipeline
python3 -m pytest pipeline/tests/          # tot hauria de sortir verd, no cal instal·lar res amb pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### On l'ajuda és més útil

**i18n / localització** — `worker/src/i18n/ui-strings.json` és la font de les cadenes de la interfície d'usuari. La revisió per part de parlants nadius de qualsevol localització no anglesa és de gran valor: detectar traduccions automàtiques maldestres, corregir problemes de RTL/maquetació, millorar casos límit de negociació d'idioma.

**Qualitat de l'OCR** — un millor preprocessament d'antics documents escanejats a màquina abans de l'OCR; un sistema d'avaluació que compari el motor de codi obert amb el fallback de Tesseract en pàgines de mostra.

**Accessibilitat** — auditar les pàgines renderitzades (`worker/src/render/`) segons WCAG; el CSP és estricte (sense `unsafe-inline`), així que les solucions han de funcionar dins d'aquest marc.

**Ergonomia de l'API** — `worker/src/routes/` — paginació, filtratge, descripció OpenAPI, clients d'exemple.

**Robustesa del pipeline** — més rutes de degradació elegant, millors informes de progrés, casos límit en la detecció de deltes (`pipeline/lib/delta.py`).

**Documentació** — `docs/20260511/` (繁體中文; `00-*` és l'índex). Les traduccions dels documents de disseny a l'anglès són benvingudes.

### Normes bàsiques

- Totes les rutes han de ser relatives — el projecte ha de ser portable entre màquines. Sense camins absoluts codificats.
- No afegiu una dependència de pip a un mòdul *principal* del pipeline. Les etapes opcionals poden utilitzar paquets opcionals, i han de degradar-se amb elegància si no hi són.
- No debiliteu la màquina d'estats només d'avanç — aquest és el sostre de cost.
- No introduïu insígnies oficials del govern dels EUA, i no afegiu res que reverteixi les redaccions originals.
- Els canvis a l'esquema de D1 afecten **dos** fitxers: `pipeline/lib/manifest_schema.sql` i `db/schema.sql`.
- Les proves han d'acompanyar el codi nou. Missatges de commit en format Conventional Commits.

Llegiu primer `CLAUDE.md` i `docs/20260511/00-*`, i després obriu un issue per discutir qualsevol canvi estructural abans del PR.

