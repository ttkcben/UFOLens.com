# GitHub — Publicación 3 de 3 · Notas de arquitectura (Discussion estilo ADR)

**Uso:** una Discussion en "Show and tell" / "Architecture", o semilla de ADR en `docs/`.
**Palabras clave:** arquitectura, ADR, máquina de estados forward-only, LLM local, Ollama, OCR, edge computing, CSP, cabeceras de seguridad, pipeline de datos, ingeniería de costes, manifest SQLite, D1, R2, KV
**Enlaces:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Por qué ufolens.com está construido así

Notas sobre las tres decisiones que dieron forma a [ufolens.com](https://www.ufolens.com) (la reconstrucción buscable y multilingüe del [archivo PURSUE de UAP](https://www.war.gov/ufo)). Comentarios / objeciones son bienvenidos.

### 1. El pipeline es una máquina de estados forward-only — a propósito

Estados: `discovered → downloaded → ocr_done → translated → published`. Un documento solo avanza, y solo cuando hay trabajo que hacer. El contenido publicado nunca se reprocesa salvo que un detector de deltas vea que el origen realmente ha cambiado.

**Por qué:** OCR + traducción son las operaciones caras, y el archivo crece con el tiempo. Un pipeline que "re-ejecuta todo por si acaso" tiene un coste ilimitado. Hacer que las transiciones hacia atrás sean imposibles hace que una factura desbocada también lo sea. El techo de coste es una propiedad del grafo de estados, no de la vigilancia del operador.

**Coste:** las migraciones de schema y el reprocesamiento intencional son deliberadamente incómodos. Trade-off aceptable.

### 2. OCR y traducción corren en un LLM local, no en una API en la nube

OCR: motor open-source, fallback Tesseract CLI. Traducción + NER: Gemma vía Ollama, en un portátil Apple Silicon.

**Por qué:** coste marginal por documento cero; reproducible (modelo + prompts fijos); y el paso de fetch tiene que correrse desde una IP residencial (el origen está detrás de Akamai Bot Manager — `curl` recibe 403), así que un portátil ya está en el loop.

**Coste:** la calidad de traducción está por debajo de un modelo frontera. Para un corpus de referencia donde el original en inglés está siempre a un clic, eso está bien. No afirmamos que las traducciones sean autoritativas.

### 3. Las dos mitades comparten exactamente una interfaz: un bundle publicado

El pipeline nunca escribe directamente en la base de datos de producción. Emite `{ SQL, manifiesto de activos, lista de purga de caché }`. "Publicar" = aplicar ese bundle hacia adelante (push del SQL a la edge SQL DB, sync de activos al object storage, purge de las claves de caché indicadas).

**Por qué:** el lado local y el lado de borde pueden evolucionar de forma independiente; el bundle es revisable; y "desplegar datos" tiene siempre la misma forma. El Worker es una app pequeña en TypeScript/Hono — CSP estricto (sin `unsafe-inline`; JSON-LD inline fijado con sha256), negociación `Accept-Language` + país→idioma, caché KV de páginas de 30 días, cron diario de mantenimiento — y nunca necesita saber cómo se hicieron los datos.

**Coste:** un cambio de schema en D1 toca dos archivos (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Seguro barato.

### No negociables, integrados en el comportamiento

- No afiliado al gobierno de EE. UU.; sin insignias oficiales.
- Las redacciones del origen se preservan, nunca se revierten.
- Vídeo atribuido a DVIDS / AARO.
- En todo el sitio: `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` — indexable por buscadores, opt-out frente a scraping de IA.

En vivo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
