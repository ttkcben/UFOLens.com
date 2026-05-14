# GitHub — Post 1 de 3 · Anuncio de lanzamiento / bloque ta o README

**Uso:** como cuerpo d'un GitHub Release, una Discusión fixada, u en a parte superior d'o README d'o repositorio.
**Parolas clau:** UAP, UFO, PURSUE archive, documentos desclasificatos, datos ubiertos, busca de texto completo, OCR, traducción automatica, LLM local, Ollama, edge computing, API publica, Hono, TypeScript, Python
**Vinclos:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — una plataforma multilingüe y con capacidat de busca ta l'archivo PURSUE UAP

**En directo:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Archivo fuent:** https://www.war.gov/ufo

`ufolens.com` torna a publicar l'archivo **PURSUE** d'o Departamento de Guerra d'os Estaus Unius de rechistros desclasificatos de UAP / UFO como una plataforma de conoiximiento: busca de texto completo, traducción automatica en tot o corpus, exploración por mapa y linia de tiempo, y una API publica JSON. Os documentos fuent son obras d'o gubierno federal d'os EE.UU. y dentro d'os EE.UU. son de dominio publico ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Este prochecto **no ye afiliato con o gubierno d'os EE.UU.**, no fa servir garra insignia oficial, y nunca no reverte as redaccions.

### Arquitectura

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

- **Coste zero d'IA en a nube por documento.** L'OCR y a traducción s'executan localment; a maquina d'estaus de nomás entabant (`discovered → downloaded → ocr_done → translated → published`) guarenzia que dengún documento no se torna a procesar si no ha cambiato.
- **O nuclio d'o pipeline no tiene dependencias de tercers** — os modulos de parsing / manifiesto / delta s'executan y preban en un Python limpio sin cosa instalada con pip; as fases d'OCR/traducción se degradan con elegancia quan os paquetz opcionals no i son.
- **O puesto web en o edge** aplica cabeceras de seguridat estrictas + CSP (sin `unsafe-inline`; o JSON-LD en linia ye fixato con sha256), negociación d'idioma a traviés de `Accept-Language` + mapeyo de país, un caché de pachina en KV de 30 días, y un cron diario de mantenimiento.
- **Actualizacions incrementals:** un detector de deltas fa un diff de l'endice fuent y nomás ninvía os cambios de tornada a o pipeline.

### Ta desenvolicadors

L'API publica en https://www.ufolens.com/api/v1 devuelve documentos y metadatos como JSON. L'acceso anonimo tiene un limite de peticions; solicite una clau ta os livels d'investigador/desenvolicador. Veiga a sección de l'API en o puesto web ta os endpoints y limites.

### Estau

Codigo completo; puesto web desplegato en https://www.ufolens.com. A base de datos de producción se plena executando o pipeline offline y publicando o paquet entabant (`cli_publish run --remote`). A documentación completa d'o disenyo se troba en `docs/20260511/`.

### Licencia / mugas

- Documentos fuent: obras d'o gubierno federal d'os Estaus Unius, dominio publico en os Estaus Unius.
- O propio codigo d'esta plataforma: veiga `LICENSE`.
- O puesto web ninvía `Tdm-Reservation: 1` y `X-Robots-Tag: noai, noimageai` — indexable por motors de busca, optato por no participar en l'entrenamiento/scraping d'IA.
- As imáchens de video s'atribuyen a DVIDS / AARO y no son reclamadas por este prochecto.

Se dan a bienplegada a os issues y PRs. Por favor, lea `CLAUDE.md` y `docs/20260511/00-*` antes d'ubrir cambios estructurals.

