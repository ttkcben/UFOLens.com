# GitHub — Publicació 3 de 3 · Notes d'arquitectura (Discussió estil ADR)

**Ús:** com a Discussió en "Show and tell" / "Architecture", o com a base per a un ADR en `docs/`.
**Paraules clau:** arquitectura, ADR, màquina d'estats de només avanç, LLM local, Ollama, OCR, computació edge, CSP, capçaleres de seguretat, pipeline de dades, enginyeria de costos, manifest SQLite, D1, R2, KV
**Hipervincles:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Per què ufolens.com està construït com està

Notes sobre les tres decisions que van donar forma a [ufolens.com](https://www.ufolens.com) (la reconstrucció cercable i multilingüe de l'[arxiu PURSUE UAP](https://www.war.gov/ufo)). Comentaris / objeccions són benvinguts.

### 1. El pipeline és una màquina d'estats de només avanç — a propòsit

Estats: `discovered → downloaded → ocr_done → translated → published`. Un document només avança, i només quan hi ha faena a fer. El contingut publicat mai es reprocessa a menys que un detector de deltes veja que la font realment ha canviat.

**Per què:** L'OCR i la traducció són les operacions costoses, i l'arxiu creix amb el temps. Un pipeline que "ho torna a executar tot per seguretat" té un cost il·limitat. Fer impossibles les transicions cap arrere fa impossible una factura descontrolada. El sostre de cost és una propietat del graf d'estats, no de la vigilància de l'operador.

**Cost:** les migracions d'esquema i el reprocessament a propòsit són deliberadament incòmodes. Un compromís acceptable.

### 2. L'OCR i la traducció s'executen en un LLM local, no en una API al núvol

OCR: motor de codi obert, alternativa Tesseract CLI. Traducció + NER: Gemma a través d'Ollama, en un portàtil Apple Silicon.

**Per què:** cost marginal zero per document; reproduïble (model i prompts fixos); i el pas de `fetch` ja ha d'executar-se des d'una IP residencial (la font està darrere d'Akamai Bot Manager — `curl` rep un 403), així que un portàtil ja està en el bucle de totes maneres.

**Cost:** la qualitat de la traducció està per davall d'un model de frontera. Per a un corpus de referència on l'anglés original sempre està a un clic de distància, això està bé. No afirmem que les traduccions siguen autoritatives.

### 3. Les dos meitats comparteixen exactament una interfície: un paquet publicat

El pipeline mai escriu directament a la base de dades de producció. Emet `{ SQL, manifest d'actius, llista de purga de caché }`. "Publicar" = aplicar eixe paquet cap avant (pujar SQL a la BD SQL edge, sincronitzar actius a l'emmagatzematge d'objectes, purgar les claus de caché anomenades).

**Per què:** el costat local i el costat edge poden evolucionar independentment; el paquet és revisable; i "desplegar dades" té sempre la mateixa forma. El Worker és una xicoteta aplicació TypeScript/Hono — CSP estricte (sense `unsafe-inline`; el JSON-LD en línia està fixat amb sha256), negociació `Accept-Language` + país→idioma, caché de pàgina KV de 30 dies, cron de manteniment diari — i mai necessita saber com es van crear les dades.

**Cost:** un canvi a l'esquema D1 afecta dos fitxers (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Una assegurança barata.

### Innegociables integrats en el comportament

- No afiliat amb el govern dels EUA; sense insígnies oficials.
- Les redaccions originals es preserven, mai es reverteixen.
- Vídeo atribuït a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` en tot el lloc — indexable per a cerques, exclòs de l'scraping d'IA.

En viu: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

