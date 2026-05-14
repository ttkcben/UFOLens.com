# GitHub — Post 3 de 3 · Notas d'arquitectura (Discusión estilo ADR)

**Uso:** como una Discusión baixo "Amostrar y contar" / "Arquitectura", u como simient ta un ADR en `docs/`.
**Parolas clau:** arquitectura, ADR, maquina d'estaus de nomás entabant, LLM local, Ollama, OCR, edge computing, CSP, cabeceras de seguridat, pipeline de datos, incheniería de costes, manifiesto SQLite, D1, R2, KV
**Vinclos:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Por qué ufolens.com ye construito como ye

Notas sobre as tres decisions que dioron forma a [ufolens.com](https://www.ufolens.com) (a reconstrucción multilingüe y con capacidat de busca de l'[archivo PURSUE UAP](https://www.war.gov/ufo)). Os comentarios / criticas son bienplegatos.

### 1. O pipeline ye una maquina d'estaus de nomás entabant — a esprés

Estaus: `discovered → downloaded → ocr_done → translated → published`. Un documento nomás abanza entabant, y nomás quan bi ha faina que fer. O conteniu publicato nunca no se torna a procesar a menos que un detector de deltas veiga que a fuent ha cambiato de verdat.

**Por qué:** L'OCR + a traducción son as operacions caras, y l'archivo creixe con o tiempo. Un pipeline que "lo torna a executar todo por si de cas" tiene un coste ilimitato. Fer que as transicions entazaga sían imposibles fa que una factura fuera de control sía imposible. O teito de coste ye una propiedat d'o grafo d'estaus, no d'a vichilancia de l'operador.

**Coste:** as migracions d'esquema y o reprocesamiento a esprés son deliberadament incomodos. Un compromís aceptable.

### 2. L'OCR y a traducción s'executan en un LLM local, no en una API en a nube

OCR: motor de codigo ubierto, fallback a Tesseract CLI. Traducción + NER: Gemma a traviés de Ollama, en un portatil Apple Silicon.

**Por qué:** coste marchinal zero por documento; reproducible (modelo + prompts fixos); y o paso de descarga ya s'ha d'executar dende una IP residencial (a fuent ye dezaga de Akamai Bot Manager — `curl` recibe un 403), asinas que un portatil ya ye en o bucle de todas trazas.

**Coste:** a calidat d'a traducción ye por debaixo d'un modelo de primera linia. Ta un corpus de referencia an l'orichinal en anglés siempre ye a un clic de distancia, ye prou. No afirmamos que as traduccions sían autoritativas.

### 3. As dos mitaz comparten exactament una interfaz: un paquet publicato

O pipeline nunca no escribe directament en a base de datos de producción. Emite `{ SQL, manifiesto d'activos, lista de purga de caché }`. "Publicar" = aplicar ixe paquet entabant (puyar o SQL a la base de datos SQL d'o edge, sincronizar os activos con l'almagazenamiento d'obchectos, purgar as claus de caché nombradas).

**Por qué:** o costato local y o costato edge pueden evolucionar independientment; o paquet se puede revisar; y "desplegar datos" tiene a mesma forma cada vegada. O Worker ye una aplicación chicota de TypeScript/Hono — CSP estricto (sin `unsafe-inline`; o JSON-LD en linia ye fixato con sha256), negociación de `Accept-Language` + país→idioma, caché de pachina en KV de 30 días, cron diario de mantenimiento — y nunca no necesita saber cómo se creyoron os datos.

**Coste:** un cambio en o esquema de D1 afecta a dos fichers (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un seguro barato.

### Puntos no negociables incorporatos en o comportamiento

- No ye afiliato con o gubierno d'os EE.UU.; denguna insignia oficial.
- As redaccions d'a fuent se conservan, nunca no se revierten.
- Video atribuito a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` en tot o puesto — indexable por motors de busca, optato por no participar en o scraping d'IA.

En directo: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
