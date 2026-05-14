# GitHub — Publicacion 3 de 3 · Nòtas d'arquitectura (Discussion estil ADR)

**Utilizar coma:** una discussion jos "Mòstra e raconta" / "Arquitectura", o una grana d'ADR per `docs/`.
**Mots clau:** arquitectura, ADR, maquina d'estats d'avançament unic, LLM local, Ollama, OCR, edge computing, CSP, entèstas de seguretat, pipeline de donadas, engenhariá de còstes, manifèst SQLite, D1, R2, KV
**Iperligams:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Perqué ufolens.com es bastit d'aquesta manièra

Nòtas sus las tres decisions qu'an format [ufolens.com](https://www.ufolens.com) (la reconstruccion cercabla e multilingüa de las [archius UAP PURSUE](https://www.war.gov/ufo)). Los comentaris / contradiccions son benvenguts.

### 1. Lo pipeline es una maquina d'estats d'avançament unic — deliberadament

Estats: `discovered → downloaded → ocr_done → translated → published`. Un document avança solament, e solament quand i a de trabalh a far. Lo contengut publicat es pas jamai tornat tractar levat se un detector de deltas vei que la font a efectivament cambiat.

**Perqué:** L'OCR e la traduccion son las operacions costosas, e las archius creisson amb lo temps. Un pipeline que "torna executar tot per seguretat" a un còst illimitat. Rendre las transicions enrèire impossiblas rend una factura fòla impossibla. Lo plafon de còst es una proprietat del graf d'estats, pas de la vigilància de l'operator.

**Còst:** las migracions d'esquèma e lo retractament intencionat son deliberadament incomòdes. Compromés acceptable.

### 2. L'OCR e la traduccion s'executan sus un LLM local, pas sus una API cloud

OCR: motor open-source, recors a la CLI Tesseract. Traduccion + NER: Gemma via Ollama, sus un ordenador portable Apple Silicon.

**Perqué:** còst marginal zèro per document; reproductible (modèl + instruccions fixes); e l'estapa de recuperacion deu ja s'executar dempuèi una IP residenciala (la font es darrièr Akamai Bot Manager — `curl` recep un 403), doncas un ordenador portable es de tot biais dins la bocla.

**Còst:** la qualitat de la traduccion es inferiora a un modèl de punta. Per un corpus de referéncia ont l'original anglés es sempre a un clic, aquò es acceptable. Pretendèm pas que las traduccions fagan autoritat.

### 3. Las doas mitats partejan exactament una sola interfàcia: un paquet publicat

Lo pipeline escriu pas jamai dirèctament dins la basa de donadas de produccion. Produsís `{ SQL, manifèst d'actius, lista de purga del cache }`. "Publicar" = aplicar aquel paquet cap avant (enviar lo SQL a la BD SQL edge, sincronizar los actius amb l'estocatge d'objèctes, purgar las claus de cache nominadas).

**Perqué:** la partida locala e la partida edge pòdon evoluir independentament; lo paquet es revisable; e "desplegar las donadas" a la meteissa forma cada còp. Lo Worker es una pichona aplicacion TypeScript/Hono — CSP estricte (pas de `unsafe-inline`; lo JSON-LD en linha es fixat amb sha256), negociacion `Accept-Language` + país→lenga, cache de pagina KV de 30 jorns, cron de mantenença jornalièr — e a pas jamai besonh de saber cossí las donadas son estadas creadas.

**Còst:** un cambiament d'esquèma D1 afècta dos fichièrs (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Una assegurança pauc costosa.

### Elements non negociables integrats al comportament

- Pas afiliat al govèrn dels EUA; pas d'insignes oficials.
- Las redaccions de la font son conservadas, jamai anulladas.
- Vidèo atribuida a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` a l'escala del site — indexable per la cèrca, desactivat per lo rasclatge IA.

En linha: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
