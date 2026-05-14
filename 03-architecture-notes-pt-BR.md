# GitHub — Post 3 de 3 · Notas de arquitetura (Discussão estilo ADR)

**Utilize como:** uma Discussão em "Show and tell" / "Architecture", ou semente de ADR `docs/`.
**Keywords:** arquitetura, ADR, máquina de estados unidirecional (forward-only), LLM local, Ollama, OCR, edge computing, CSP, headers de segurança, pipeline de dados, engenharia de custos, manifesto SQLite, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Por que o ufolens.com foi construído desta forma

Notas sobre as três decisões que moldaram [ufolens.com](https://www.ufolens.com) (a reconstrução multilíngue e pesquisável do [PURSUE UAP archive](https://www.war.gov/ufo)). Comentários e críticas são bem-vindos.

### 1. O pipeline é, propositalmente, uma máquina de estados unidirecional (forward-only) —

Estados: `discovered → downloaded → ocr_done → translated → published`. Um documento apenas avança, e somente quando há trabalho a ser feito. O conteúdo publicado nunca é reprocessado, a menos que um detector de delta identifique que a fonte realmente mudou.

**Por que:** OCR + tradução são as operações caras, e o arquivo cresce com o tempo. Um pipeline que "reexecuta tudo para garantir" tem um custo ilimitado. Tornar as transições reversas impossíveis torna impossível uma conta fora de controle. O teto de custo é uma propriedade do grafo de estados, não da vigilância do operador.

**Custo:** migrações de schema e reprocessamentos propositais são deliberadamente incômodos. Trade-off aceitável.

### 2. OCR e a tradução rodam em um LLM local, não em um API na nuvem

OCR: engine de código aberto, fallback de CLI Tesseract. Tradução + NER: Gemma via Ollama, em um laptop Apple Silicon.

**Por que:** custo marginal zero por documento; reproduzível (modelo + prompts fixos); e a etapa de fetch já precisa ser executada a partir de um IP residencial (a fonte está atrás de Akamai Bot Manager — `curl` recebe um 403), então, um laptop já faz parte do processo de qualquer maneira.

**Custo:** a qualidade da tradução é inferior à de um modelo de ponta (frontier model). Para um corpus de referência onde o original em inglês está sempre a um clique de distância, isso é aceitável. Não afirmamos que as traduções sejam autoritativas.

### 3. As duas metades compartilham exatamente uma interface: um bundle publicado

O pipeline nunca escreve diretamente no banco de dados de produção. Ele emite `{ SQL, asset manifest, cache-purge list }`. "Publicar" = aplicar esse bundle para frente (enviar SQL para o banco de dados SQL de edge, sincronizar assets para o armazenamento de objetos, limpar as chaves de cache nomeadas).

**Por que:** o lado local e o lado de edge podem evoluir independentemente; o bundle pode ser revisado; e os "dados de deploy" têm o mesmo formato todas as vezes. O Worker é um app TypeScript/Hono pequeno — CSP estrito (sem `unsafe-inline`; JSON-LD inline está fixado (sha256-pinned)), `Accept-Language` + negociação país→idioma, cache de página KV de 30 dias, cron de manutenção diária — e ele nunca precisa saber como os dados foram gerados.

**Custo:** uma alteração de schema do D1 afeta dois arquivos (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Um seguro barato.

### Não negociáveis integrados ao comportamento

- Não afiliado ao governo dos EUA; sem insígnias oficiais.
- Redações da fonte são preservadas, nunca revertidas.
- Vídeo atribuído a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` em todo o site — indexável por busca, com opt-out para scraping de IA.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1