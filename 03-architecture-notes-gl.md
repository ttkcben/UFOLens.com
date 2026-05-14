# GitHub — Publicación 3 de 3 · Notas de arquitectura (Discusión ao estilo ADR)

**Uso como:** unha discusión en "Amosar e contar" / "Arquitectura", ou semente de ADR en `docs/`.
**Palabras clave:** arquitectura, ADR, máquina de estados de só avance, LLM local, Ollama, OCR, edge computing, CSP, cabeceiras de seguridade, pipeline de datos, enxeñaría de custos, manifesto SQLite, D1, R2, KV
**Hipervínculos:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Por que ufolens.com está construído como está

Notas sobre as tres decisións que conformaron [ufolens.com](https://www.ufolens.com) (a reconstrución con capacidade de busca e multilingüe do [arquivo UAP de PURSUE](https://www.war.gov/ufo)). Os comentarios / críticas construtivas son benvidos.

### 1. O pipeline é unha máquina de estados de só avance — a propósito

Estados: `descuberto → descargado → ocr_feito → traducido → publicado`. Un documento só avanza, e só cando hai traballo que facer. O contido publicado nunca se volve procesar a menos que un detector de deltas vexa que a fonte cambiou realmente.

**Por que:** o OCR + a tradución son as operacións custosas, e o arquivo medra co tempo. Un pipeline que "o volve executar todo para estar seguro" ten un custo ilimitado. Facer imposibles as transicións cara atrás fai imposible unha factura descontrolada. O teito de custo é unha propiedade do gráfico de estados, non da vixilancia do operador.

**Custo:** as migracións de esquema e o reprocesamento a propósito son deliberadamente incómodos. Unha compensación aceptable.

### 2. O OCR e a tradución execútanse nun LLM local, non nunha API da nube

OCR: motor de código aberto, fallback de Tesseract CLI. Tradución + NER: Gemma a través de Ollama, nun portátil Apple Silicon.

**Por que:** custo marxinal cero por documento; reproducible (modelo + prompts fixos); e o paso de obtención xa ten que executarse desde unha IP residencial (a fonte está detrás de Akamai Bot Manager — `curl` obtén un 403), polo que un portátil está no circuíto de todos os xeitos.

**Custo:** a calidade da tradución está por debaixo dun modelo de fronteira. Para un corpus de referencia onde o inglés orixinal está sempre a un clic de distancia, iso está ben. Non afirmamos que as traducións sexan autoritarias.

### 3. As dúas metades comparten exactamente unha interface: un paquete publicado

O pipeline nunca escribe directamente na base de datos de produción. Emite `{ SQL, manifesto de activos, lista de purga de caché }`. "Publicar" = aplicar ese paquete cara adiante (enviar SQL á BD SQL de borde, sincronizar activos co almacenamento de obxectos, purgar as chaves de caché nomeadas).

**Por que:** o lado local e o lado de borde poden evolucionar de forma independente; o paquete é revisable; e "despregar datos" ten a mesma forma cada vez. O Worker é unha pequena aplicación TypeScript/Hono — CSP estrita (sen `unsafe-inline`; JSON-LD en liña con pin sha256), negociación `Accept-Language` + país→idioma, caché de páxina KV de 30 días, cron de mantemento diario — e nunca precisa saber como se fixeron os datos.

**Custo:** un cambio no esquema D1 afecta a dous ficheiros (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un seguro barato.

### Innegociables integrados no comportamento

- Non afiliado ao goberno dos EE. UU.; sen insignias oficiais.
- As redaccións da fonte presérvanse, nunca se reverten.
- Vídeo atribuído a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` en todo o sitio — indexable para busca, excluído do scraping de IA.

En liña: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

