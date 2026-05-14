# GitHub — Publicació 2 de 3 · Crida a contribucions / "bons primers issues"

**Ús:** com a Discussió fixada ("Contribucions i bons primers issues") o una introducció a CONTRIBUTING.md.
**Paraules clau:** codi obert, contribuir, bon primer issue, i18n, localització, OCR, Python, TypeScript, Vitest, pytest, accessibilitat, UAP, dades obertes
**Hipervincles:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuir a ufolens.com

[ufolens.com](https://www.ufolens.com) converteix l'[arxiu PURSUE UAP](https://www.war.gov/ufo) del Departament de Guerra dels EUA en una plataforma multilingüe i cercable amb una [API pública](https://www.ufolens.com/api/v1). Són dos meitats — un pipeline d'ingestió local en Python (`pipeline/`) i una aplicació edge en TypeScript/Hono (`worker/`) — que es troben en una única interfície: un paquet publicat de SQL + actius.

No necessiteu cap credencial del núvol per a contribuir. Els mòduls principals del pipeline només usen la llibreria estàndard i les proves del Worker s'executen contra un emmagatzematge en memòria.

### Configuració

```bash
# pipeline
python3 -m pytest pipeline/tests/          # tot hauria d'estar en verd, no cal instal·lar amb pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### On l'ajuda és més útil

**i18n / localització** — `worker/src/i18n/ui-strings.json` és la font de les cadenes de la IU. La revisió per part d'un parlant natiu de qualsevol localització no anglesa és de gran valor: detectar resultats de traducció automàtica poc naturals, corregir problemes de RTL/disseny, millorar casos extrems de negociació d'idioma.

**Qualitat de l'OCR** — millor preprocessament d'antics escanejats mecanografiats abans de l'OCR; un entorn d'avaluació que compare el motor de codi obert amb l'alternativa Tesseract en pàgines de mostra.

**Accessibilitat** — auditar les pàgines renderitzades (`worker/src/render/`) contra WCAG; el CSP és estricte (sense `unsafe-inline`), així que les solucions han de funcionar dins d'eixe marc.

**Ergonomia de l'API** — `worker/src/routes/` — paginació, filtrat, descripció OpenAPI, clients d'exemple.

**Robustesa del pipeline** — més rutes de degradació elegant, millors informes de progrés, casos extrems en la detecció de deltes (`pipeline/lib/delta.py`).

**Documentació** — `docs/20260511/` (繁體中文; `00-*` és l'índex). Les traduccions dels documents de disseny a l'anglés són benvingudes.

### Regles bàsiques

- Totes les rutes relatives — el projecte ha de ser portable entre màquines. No hi ha rutes absolutes codificades.
- No afegiu una dependència de pip a un mòdul *principal* del pipeline. Les etapes opcionals poden utilitzar paquets opcionals, i han de degradar-se amb gràcia sense ells.
- No debiliteu la màquina d'estats de només avanç — eixe és el límit de cost.
- No introduïu insígnies oficials del govern dels EUA, i no afegiu res que reverta les redaccions originals.
- Els canvis a l'esquema D1 afecten **dos** fitxers: `pipeline/lib/manifest_schema.sql` i `db/schema.sql`.
- Proves amb el codi nou. Missatges de commit convencionals.

Llegiu primer `CLAUDE.md` i `docs/20260511/00-*`, i després obriu un issue per a discutir qualsevol canvi estructural abans del PR.

