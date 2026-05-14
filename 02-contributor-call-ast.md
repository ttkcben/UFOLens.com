# GitHub — Publicación 2 de 3 · Llamada a collaboradores / "bonos primeros issues"

**Usu como:** un alderique afitáu ("Contribuciones y bonos primeros issues") o una introducción en CONTRIBUTING.md.
**Pallabres clave:** códigu abiertu, contribución, bon primer issue, i18n, llocalización, OCR, Python, TypeScript, Vitest, pytest, accesibilidá, UAP, datos abiertos
**Hiperenllaces:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuyendo a ufolens.com

[ufolens.com](https://www.ufolens.com) convierte l'[archivu PURSUE UAP](https://www.war.gov/ufo) del Departamentu de Guerra de los EE.XX. nuna plataforma multillingüe y con capacidá de busca con una [API pública](https://www.ufolens.com/api/v1). Son dos metaes — un pipeline d'inxestión local en Python (`pipeline/`) y una app nel borde con TypeScript/Hono (`worker/`) — que s'atopen nuna única interfaz: un paquete publicáu de SQL + activos.

Nun precises credenciales na nube pa contribuyir. Los módulos principales del pipeline son solo de la llibrería estándar y les pruebes del Worker execútense contra un almacenamientu en memoria.

### Configuración

```bash
# pipeline
python3 -m pytest pipeline/tests/          # debería tar too en verde, nun se precisa instalar con pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Onde l'ayuda ye más preséu

**i18n / llocalización** — `worker/src/i18n/ui-strings.json` ye la fonte de les cadenes de la interfaz d'usuariu. La revisión por falantes nativos de cualquier llocalización que nun seya l'inglés ye de gran valor: atopar traducciones automátiques estrañes, iguar problemes de RTL/diseñu, ameyorar casos estremos de negociación d'idioma.

**Calidá del OCR** — meyor preprocesamientu d'escaneos antiguos escritos a máquina enantes del OCR; un arnés d'evaluación que compare'l motor de códigu abiertu col fallback de Tesseract en páxines de muestra.

**Accesibilidá** — auditar les páxines renderizaes (`worker/src/render/`) contra les WCAG; el CSP ye estrictu (ensin `unsafe-inline`), polo que les soluciones tienen de funcionar dientro d'esi marcu.

**Ergonomía de la API** — `worker/src/routes/` — paxinación, filtráu, descripción OpenAPI, veceros d'exemplu.

**Robustez del pipeline** — más rutes de degradación con elegancia, meyor informe de progresu, casos estremos de detección de deltes (`pipeline/lib/delta.py`).

**Documentación** — `docs/20260511/` (繁體中文; `00-*` ye l'índiz). Les traducciones de los documentos de diseñu al inglés son bienveníes.

### Regles básiques

- Toles rutes relatives — el proyeutu tien de ser portable ente máquines. Nun hai rutes absolutes codificaes.
- Nun amiestes una dependencia de pip a un módulu *principal* del pipeline. Les etapes opcionales pueden usar paquetes opcionales y tienen de degradar con elegancia ensin ellos.
- Nun debilites la máquina d'estaos de solo meyora — esa ye la llende de costu.
- Nun amiestes insinies oficiales del gobiernu de los EE.XX. y nun amiestes nada que revierta les censures fonte.
- Los cambeos nel esquema D1 toquen **dos** ficheros: `pipeline/lib/manifest_schema.sql` y `db/schema.sql`.
- Pruebes col códigu nuevu. Mensaxes de commit convencionales.

Llee `CLAUDE.md` y `docs/20260511/00-*` primero, y depués abre un issue p'aldericar cualquier cosa estructural enantes del PR.

