# GitHub — Publicació 3 de 3 · Notes d'arquitectura (discussió estil ADR)

**Ús:** com a discussió a "Demostració i explicació" / "Arquitectura", o com a base per a un ADR a `docs/`.
**Paraules clau:** arquitectura, ADR, màquina d'estats només d'avanç, LLM local, Ollama, OCR, edge computing, CSP, capçaleres de seguretat, pipeline de dades, enginyeria de costos, manifest SQLite, D1, R2, KV
**Enllaços:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Per què ufolens.com està fet d'aquesta manera

Notes sobre les tres decisions que van donar forma a [ufolens.com](https://www.ufolens.com) (la reconstrucció cercable i multilingüe de l'[arxiu PURSUE UAP](https://www.war.gov/ufo)). Comentaris i crítiques són benvinguts.

### 1. El pipeline és una màquina d'estats només d'avanç, a propòsit

Estats: `descobert → descarregat → ocr_fet → traduït → publicat`. Un document només avança, i només quan hi ha feina a fer. El contingut publicat no es reprocessa mai a menys que un detector de deltes vegi que la font ha canviat realment.

**Per què:** L'OCR i la traducció són les operacions costoses, i l'arxiu creix amb el temps. Un pipeline que "ho torna a executar tot per seguretat" té un cost il·limitat. Fer impossibles les transicions cap enrere fa impossible una factura descontrolada. El sostre de cost és una propietat del gràfic d'estats, no de la vigilància de l'operador.

**Cost:** les migracions d'esquema i el reprocessament intencionat són deliberadament incòmodes. Un intercanvi acceptable.

### 2. L'OCR i la traducció s'executen en un LLM local, no en una API al núvol

OCR: motor de codi obert, fallback a Tesseract CLI. Traducció + NER: Gemma via Ollama, en un portàtil Apple Silicon.

**Per què:** cost marginal zero per document; reproduïble (model + prompts fixos); i el pas de descàrrega ja s'ha d'executar des d'una IP residencial (la font està darrere d'Akamai Bot Manager — `curl` rep un 403), així que un portàtil ja forma part del bucle de totes maneres.

**Cost:** la qualitat de la traducció és inferior a la d'un model de frontera. Per a un corpus de referència on l'anglès original és sempre a un clic de distància, això és acceptable. No afirmem que les traduccions siguin autoritatives.

### 3. Les dues meitats comparteixen exactament una interfície: un paquet publicat

El pipeline no escriu mai directament a la base de dades de producció. Emet `{ SQL, manifest d'actius, llista de purga de cau }`. "Publicar" = aplicar aquest paquet (pujar l'SQL a la base de dades SQL edge, sincronitzar els actius a l'emmagatzematge d'objectes, purgar les claus de cau anomenades).

**Per què:** el costat local i el costat edge poden evolucionar de manera independent; el paquet és revisable; i "desplegar dades" té la mateixa forma cada vegada. El Worker és una petita aplicació TypeScript/Hono — CSP estricte (sense `unsafe-inline`; el JSON-LD incrustat està fixat amb sha256), negociació `Accept-Language` + país→idioma, cau de pàgines a KV de 30 dies, cron de manteniment diari — i no necessita saber mai com es van crear les dades.

**Cost:** un canvi a l'esquema de D1 afecta dos fitxers (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Una assegurança barata.

### Innegociables integrats en el comportament

- No afiliat al govern dels EUA; sense insígnies oficials.
- Les redaccions originals es preserven, mai es reverteixen.
- Vídeo atribuït a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` a tot el lloc — indexable per cerca, exclòs de l'extracció per part d'IA.

En directe: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

