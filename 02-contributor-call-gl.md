# GitHub — Publicación 2 de 3 · Chamada a colaboradores / "boas primeiras issues"

**Uso como:** unha discusión fixada ("Contribuír e boas primeiras issues") ou unha introdución a CONTRIBUTING.md.
**Palabras clave:** código aberto, contribuír, boa primeira issue, i18n, localización, OCR, Python, TypeScript, Vitest, pytest, accesibilidade, UAP, datos abertos
**Hipervínculos:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuír a ufolens.com

[ufolens.com](https://www.ufolens.com) converte o [arquivo UAP de PURSUE](https://www.war.gov/ufo) do Departamento de Guerra dos EE. UU. nunha plataforma con capacidade de busca e multilingüe cunha [API pública](https://www.ufolens.com/api/v1). Son dúas metades — un pipeline de inxestión local en Python (`pipeline/`) e unha aplicación de borde en TypeScript/Hono (`worker/`) — que se atopan nunha única interface: un paquete publicado de SQL + activos.

Non precisa ningunha credencial da nube para contribuír. Os módulos principais do pipeline son só da biblioteca estándar e as probas do Worker execútanse contra un almacenamento en memoria.

### Configuración

```bash
# pipeline
python3 -m pytest pipeline/tests/          # debería estar todo en verde, sen necesidade de instalar con pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Onde a axuda é máis útil

**i18n / localización** — `worker/src/i18n/ui-strings.json` é a fonte das cadeas da interface de usuario. A revisión por parte dun falante nativo de calquera localidade que non sexa inglés é de gran valor: detectar saídas de máquina torpes, corrixir problemas de RTL/deseño, mellorar casos límite na negociación de idiomas.

**Calidade do OCR** — mellor pre-procesamento de escaneos antigos mecanografados antes do OCR; un arnés de avaliación que compare o motor de código aberto co fallback de Tesseract en páxinas de mostra.

**Accesibilidade** — auditar as páxinas renderizadas (`worker/src/render/`) contra WCAG; a CSP é estrita (sen `unsafe-inline`), polo que as solucións deben funcionar dentro diso.

**Ergonomía da API** — `worker/src/routes/` — paxinación, filtrado, descrición OpenAPI, clientes de exemplo.

**Robustez do pipeline** — máis rutas de degradación elegante, mellor informe de progreso, casos límite na detección de deltas (`pipeline/lib/delta.py`).

**Documentación** — `docs/20260511/` (繁體中文; `00-*` é o índice). As traducións dos documentos de deseño ao inglés son benvidas.

### Regras básicas

- Todas as rutas relativas — o proxecto debe ser portable entre máquinas. Non hai rutas absolutas codificadas.
- Non engada unha dependencia de pip a un módulo *núcleo* do pipeline. As etapas opcionais poden usar paquetes opcionais, e deben degradarse con elegancia sen eles.
- Non debilite a máquina de estados de só avance — ese é o teito de custo.
- Non introduza insignias oficiais do goberno dos EE. UU., e non engada nada que reverta as redaccións da fonte.
- Os cambios no esquema D1 afectan a **dous** ficheiros: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Probas con código novo. Mensaxes de commit convencionais.

Lea `CLAUDE.md` e `docs/20260511/00-*` primeiro, e logo abra unha issue para discutir calquera cousa estrutural antes do PR.

