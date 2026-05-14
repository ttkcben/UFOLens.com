# GitHub — Publicação 3 de 3 · Notas de arquitetura (Discussão ao estilo ADR)

**Utilizar como:** uma Discussão em "Show and tell" / "Architecture", ou semente de ADR `docs/`.
**Keywords:** arquitetura, ADR, máquina de estados apenas progressiva, LLM local, Ollama, OCR, edge computing, CSP, headers de segurança, pipeline de dados, engenharia de custos, manifesto SQLite, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Porque é que o ufolens.com foi construído desta forma

Notas sobre as três decisões que moldaram o [ufolens.com](https://www.ufolens.com) (a reconstrução multilíngue e pesquisável do [PURSUE UAP archive](https://www.war.gov/ufo)). Comentários / críticas são bem-vindos.

### 1. O pipeline é, propositadamente, uma máquina de estados apenas progressiva —

Estados: `discovered → downloaded → ocr_done → translated → published`. Um documento apenas avança, e apenas quando há trabalho a realizar. O conteúdo publicado nunca é reprocessado, a menos que um detetor de deltas verifique que a fonte realmente mudou.

**Porquê:** OCR + tradução são as operações dispendiosas, e o arquivo cresce com o tempo. Um pipeline que "reexecuta tudo para garantir" tem um custo ilimitado. Tornar as transições retrocedentes impossíveis torna impossível uma fatura descontrolada. O teto de custos é uma propriedade do grafo de estados, e não da vigilância do operador.

**Custo:** as migrações de esquema e o reprocessamento propositado são deliberadamente incómodos. Trade-off aceitável.

### 2. OCR e a tradução correm num LLM local, não num API na cloud

OCR: motor open-source, fallback de CLI Tesseract. Tradução + NER: Gemma via Ollama, num portátil Apple Silicon.

**Porquê:** custo marginal zero por documento; reproduzível (modelo + prompts fixos); e a etapa de recolha (fetch) já tem de correr a partir de um IP residencial (a fonte está atrás de Akamai Bot Manager — `curl` recebe um 403), pelo que um portátil já faz parte do processo de qualquer forma.

**Custo:** a qualidade da tradução está abaixo de um modelo de vanguarda (frontier model). Para um corpus de referência onde o original em inglês está sempre a um clique de distância, isso é aceitável. Não pretendemos que as traduções sejam autoritativas.

### 3. As duas metades partilham exatamente uma interface: um bundle publicado

O pipeline nunca escreve diretamente na base de dados de produção. Ele emite `{ SQL, asset manifest, cache-purge list }`. "Publicar" = aplicar esse bundle progressivamente (enviar SQL para a DB SQL de edge, sincronizar assets para o armazenamento de objetos, limpar as chaves de cache nomeadas).

**Porquê:** o lado local e o lado de edge podem evoluir independentemente; o bundle pode ser revisto; e os "dados de deploy" têm sempre a mesma estrutura. O Worker é uma pequena app TypeScript/Hono — strict CSP (sem `unsafe-inline`; JSON-LD inline está fixado sha256-pinned), `Accept-Language` + negociação país→língua, cache de páginas KV de 30 dias, cron de manutenção diária — e nunca precisa de saber como os dados foram criados.

**Custo:** uma alteração de esquema D1 afeta dois ficheiros (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Um seguro barato.

### Não negociáveis integrados no comportamento

- Não afiliado ao governo dos EUA; sem insígnias oficiais.
- As redações da fonte são preservadas, nunca revertidas.
- Vídeo atribuído a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` em todo o site — indexável por pesquisa, com opt-out para scraping de IA.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1