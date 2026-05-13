# GitHub — Publicación 2 de 3 · Llamada a colaboradores / "good first issues"

**Uso:** una Discussion fijada ("Contributing & good first issues") o la introducción de CONTRIBUTING.md.
**Palabras clave:** open source, contribuciones, good first issue, i18n, localización, OCR, Python, TypeScript, Vitest, pytest, accesibilidad, UAP, datos abiertos
**Enlaces:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Cómo contribuir a ufolens.com

[ufolens.com](https://www.ufolens.com) convierte el [archivo PURSUE de UAP](https://www.war.gov/ufo) del Departamento de Guerra de EE. UU. en una plataforma buscable y multilingüe con [API pública](https://www.ufolens.com/api/v1). Son dos mitades — un pipeline de ingestión Python local (`pipeline/`) y una app de borde TypeScript/Hono (`worker/`) — que se encuentran en una única interfaz: un bundle publicado de SQL + activos.

No necesitas credenciales de nube para contribuir. Los módulos núcleo del pipeline son solo stdlib y los tests del Worker corren contra almacenamiento en memoria.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # debería pasar todo, sin pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Dónde más ayuda hace falta

**i18n / localización** — `worker/src/i18n/ui-strings.json` es la fuente de strings de UI. La revisión por hablantes nativos de cualquier locale no-inglés tiene mucho valor: detectar salidas mecánicas torpes, arreglar problemas de RTL/maquetación, mejorar casos límite de negociación de idioma.

**Calidad de OCR** — mejor preprocesado de escaneos antiguos a máquina antes del OCR; un harness de evaluación que compare el motor open-source frente al fallback Tesseract sobre páginas de muestra.

**Accesibilidad** — auditar las páginas renderizadas (`worker/src/render/`) contra WCAG; el CSP es estricto (sin `unsafe-inline`), así que las soluciones deben funcionar dentro de esa restricción.

**Ergonomía de la API** — `worker/src/routes/` — paginación, filtros, descripción OpenAPI, clientes de ejemplo.

**Robustez del pipeline** — más caminos de degradación elegante, mejor reporte de progreso, casos límite de detección de deltas (`pipeline/lib/delta.py`).

**Documentación** — `docs/20260511/` (chino tradicional; `00-*` es el índice). Traducciones al inglés de los documentos de diseño son bienvenidas.

### Reglas básicas

- Todas las rutas relativas — el proyecto debe ser portable entre máquinas. Sin rutas absolutas hardcodeadas.
- No agregues una dependencia pip a un módulo *núcleo* del pipeline. Las etapas opcionales pueden usar paquetes opcionales, pero deben degradar con elegancia sin ellos.
- No debilites la máquina de estados forward-only — ese es el techo de coste.
- No introduzcas insignias oficiales del gobierno de EE. UU., y no agregues nada que revierta las redacciones del origen.
- Los cambios de schema en D1 tocan **dos** archivos: `pipeline/lib/manifest_schema.sql` y `db/schema.sql`.
- Tests con código nuevo. Mensajes de commit en formato Conventional Commits.

Lee `CLAUDE.md` y `docs/20260511/00-*` primero, luego abre un issue para discutir cualquier cambio estructural antes del PR.
