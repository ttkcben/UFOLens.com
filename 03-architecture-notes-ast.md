# GitHub — Publicación 3 de 3 · Notes d'arquiteutura (alderique estilu ADR)

**Usu como:** un alderique en "Amosar y cuntar" / "Arquiteutura", o como gueta pa un ADR en `docs/`.
**Pallabres clave:** arquiteutura, ADR, máquina d'estaos de solo meyora, LLM local, Ollama, OCR, edge computing, CSP, testeres de seguridá, pipeline de datos, inxeniería de costos, manifiestu SQLite, D1, R2, KV
**Hiperenllaces:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Por qué ufolens.com ta construyíu como ta

Notes sobre les tres decisiones que dieron forma a [ufolens.com](https://www.ufolens.com) (la reconstrucción multillingüe y con capacidá de busca del [archivu PURSUE UAP](https://www.war.gov/ufo)). Comentarios / crítiques son bienveníos.

### 1. El pipeline ye una máquina d'estaos de solo meyora — a costa fecha

Estaos: `descubiertu → descargáu → ocr_fechu → traducíu → publicáu`. Un documentu solo avanza, y solo cuando hai trabayu que facer. El conteníu publicáu nunca se reprocesa sacantes qu'un detector de deltes vea que la fonte camudó realmente.

**Por qué:** L'OCR + la traducción son les operaciones cares, y l'archivu medra col tiempu. Un pipeline que "re-executa too pa tar seguru" tien un costu ilimitáu. Facer imposibles les transiciones p'atrás fai imposible una factura descontrolada. La llende de costu ye una propiedá del gráficu d'estaos, non de la vixilancia del operador.

**Costu:** les migraciones d'esquema y el reprocesamientu a costa fecha son deliberadamente incómodes. Un compromisu aceptable.

### 2. L'OCR y la traducción execútense nun LLM local, non nuna API na nube

OCR: motor de códigu abiertu, fallback a Tesseract CLI. Traducción + NER: Gemma vía Ollama, nun portátil Apple Silicon.

**Por qué:** costu marxinal cero por documentu; reproducible (modelu + prompts fixos); y el pasu de captura yá tien d'executase dende una IP residencial (la fonte ta detrás d'Akamai Bot Manager — `curl` recibe un 403), asina qu'un portátil yá ta nel procesu de toes maneres.

**Costu:** la calidá de la traducción ta per debaxo d'un modelu de frontera. Pa un corpus de referencia onde l'orixinal n'inglés ta siempres a un clic de distancia, eso ta bien. Nun afirmamos que les traducciones sían autoritatives.

### 3. Les dos metaes comparten esautamente una interfaz: un paquete publicáu

El pipeline nunca escribe direutamente na base de datos de producción. Emite `{ SQL, manifiestu d'activos, llista de purga de caché }`. "Publicar" = aplicar esi paquete p'alantre (unviar SQL a la BBDD SQL nel borde, sincronizar activos col almacenamientu d'oxetos, purgar les claves de caché nomaes).

**Por qué:** el llau local y el llau del borde pueden evolucionar de forma independiente; el paquete ye revisable; y "desplegar datos" tien la mesma forma siempres. El Worker ye una pequeña app de TypeScript/Hono — CSP estrictu (ensin `unsafe-inline`; el JSON-LD en llinia ta afitáu con sha256), negociación d'`Accept-Language` + país→idioma, caché de páxina en KV de 30 díes, cron de caltenimientu diariu — y nunca precisa saber cómo se xeneraron los datos.

**Costu:** un cambéu nel esquema de D1 toca dos ficheros (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Un seguru baratu.

### Innegociables integraos nel comportamientu

- Nun ta afiliáu col gobiernu de los EE.XX.; ensin insinies oficiales.
- Les censures fonte presérvense, nunca se revierten.
- Videu atribuyíu a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` en tol sitiu — indexable en buscadores, escluyíu del scraping d'IA.

En direutu: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

