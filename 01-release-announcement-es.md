# GitHub — Publicación 1 de 3 · Bloque de anuncio para Release / README

**Uso:** como cuerpo de un GitHub Release, una Discussion fijada, o la parte superior del README del repo.
**Palabras clave:** UAP, OVNI, archivo PURSUE, documentos desclasificados, datos abiertos, búsqueda de texto completo, OCR, traducción automática, LLM local, Ollama, edge computing, API pública, Hono, TypeScript, Python
**Enlaces:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — una plataforma multilingüe y buscable para el archivo PURSUE de UAP

**Sitio en vivo:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Archivo de origen:** https://www.war.gov/ufo

`ufolens.com` republica el archivo **PURSUE** del Departamento de Guerra de EE. UU., que contiene registros desclasificados de UAP / OVNI, como una plataforma de conocimiento: búsqueda de texto completo en todo el corpus, traducción automática, exploración por mapa y línea de tiempo, y una API JSON pública. Los documentos de origen son obras del gobierno federal de EE. UU. y, dentro de los Estados Unidos, son de dominio público ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Este proyecto **no está afiliado al gobierno de EE. UU.**, no usa ninguna insignia oficial y nunca revierte las redacciones.

### Arquitectura

```
Máquina local (Apple Silicon, IP residencial)        Red de borde
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, núcleo solo con stdlib)     worker/  (TypeScript, Hono.js)
  fetch → OCR → traducir → publicar (forward-only)     /{lang}/...   páginas
  OCR: motor open-source (fallback Tesseract CLI)      /api/v1/...   API pública
  traducir / NER: LLM local (Gemma vía Ollama)         /admin        consola operativa
  estado: manifest SQLite                            respaldado por: edge SQL DB, object
        │                                              storage (PDF originales), KV cache
        └── publica un bundle: SQL + manifiesto de activos + lista de purga ──┘
```

- **Coste de IA en la nube por documento: cero.** OCR y traducción se ejecutan localmente; la máquina de estados forward-only (`discovered → downloaded → ocr_done → translated → published`) garantiza que ningún documento se reprocesa salvo que haya cambiado.
- **El núcleo del pipeline no tiene dependencias de terceros** — los módulos de parsing / manifest / delta corren y se prueban con un Python limpio sin nada instalado vía pip; las etapas de OCR/traducción degradan con elegancia cuando faltan paquetes opcionales.
- **El sitio de borde** aplica cabeceras de seguridad estrictas + CSP (sin `unsafe-inline`; JSON-LD inline fijado con sha256), negociación de idioma vía `Accept-Language` + mapeo por país, un caché KV de páginas de 30 días, y un cron diario de mantenimiento.
- **Actualizaciones incrementales:** un detector de deltas hace diff sobre el índice de origen y solo realimenta los cambios al pipeline.

### Para desarrolladores

La API pública en https://www.ufolens.com/api/v1 devuelve documentos y metadatos en JSON. El acceso anónimo está limitado por tasa; pide una key para los niveles researcher/developer. Endpoints y límites en la sección API del sitio.

### Estado

Código completo; sitio desplegado en https://www.ufolens.com. La base de datos en producción se llena ejecutando el pipeline offline y publicando el bundle hacia adelante (`cli_publish run --remote`). Toda la documentación de diseño vive en `docs/20260511/`.

### Licencia / límites

- Documentos de origen: obras del gobierno federal de EE. UU., dominio público dentro de Estados Unidos.
- Código propio de esta plataforma: ver `LICENSE`.
- El sitio envía `Tdm-Reservation: 1` y `X-Robots-Tag: noai, noimageai` — indexable por buscadores, exclusión opt-out frente a entrenamiento / scraping de IA.
- El material en vídeo se atribuye a DVIDS / AARO y este proyecto no reclama derechos sobre él.

Issues y PRs bienvenidos. Por favor lee `CLAUDE.md` y `docs/20260511/00-*` antes de abrir cambios estructurales.
