# GitHub — Publicação 2 de 3 · Chamada para colaboradores / "primeira issues ideal"

**Utilizar como:** uma Discussão fixada ("Contribuir e primeira issues ideal") ou como introdução ao CONTRIBUTING.md.
**Keywords:** open source, contribuição, good first issue, i18n, localização, OCR, Python, TypeScript, Vitest, pytest, acessibilidade, UAP, dados abertos
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuir para o ufolens.com

O [ufolens.com](https://www.ufolens.com) transforma o [PURSUE UAP arquivo](https://www.war.gov/ufo) do Departamento de Guerra dos EUA numa plataforma multilíngue e pesquisável com um [ API público](https://www.ufolens.com/api/v1). Divide-se em duas partes — um pipeline de ingestão Python local (`pipeline/`) e uma aplicação edge TypeScript/Hono (`worker/`) — que se encontram numa única interface: um pacote de SQL + assets publicado.

Não são necessárias credenciais de cloud para contribuir. Os módulos core do pipeline baseiam-se apenas na stdlib e os testes do Worker são executados contra armazenamento em memória.

### Configuração

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Onde a ajuda é mais útil

**i18n / localização** — `worker/src/i18n/ui-strings.json` é a fonte das strings de UI. A revisão por falantes nativos de qualquer locale que não seja inglês é extremamente valiosa: detetar resultados estranhos de tradução automática, corrigir RTL/issues de layout, melhorar casos limite (edge cases) de negociação de idioma.

**Qualidade OCR** — melhor pré-processamento de digitalizações antigas de máquina de escrever antes de OCR; estrutura de avaliação (harness) comparando o motor open-source com o fallback Tesseract em páginas de amostra.

**Acessibilidade** — auditar as páginas renderizadas (`worker/src/render/`) face ao WCAG; o CSP é rigoroso (sem `unsafe-inline`), pelo que as soluções devem funcionar dentro desse limite.

**Ergonomia API** — `worker/src/routes/` — paginação, filtragem, descrição OpenAPI, clientes de exemplo.

**Robustez do pipeline** — caminhos de degradação mais suaves (graceful degradation), melhor reporte de progresso, casos limite de deteção de delta (`pipeline/lib/delta.py`).

**Documentação** — `docs/20260511/` (繁體中文; `00-*` é o índice). Traduções dos documentos de design para inglês são bem-vindas.

### Regras básicas

- Todos os caminhos relativos — o projeto devem ser portáteis entre máquinas. Não utilizar caminhos absolutos hardcoded.
- Não adicione dependências pip a um módulo *core* do pipeline. Etapas opcionais podem utilizar pacotes opcionais e devem degradar-se suavemente na ausência destes.
- Não fragilize a máquina de estados apenas progressiva (forward-only) —, pois esta é o limite de custo.
- Não introduza insígnias oficiais do governo dos EUA e não adicione nada que reverta as redações (ocultações) da fonte.
- Alterações ao esquema D1 afetam **dois** ficheiros: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Inclua testes com código novo. Mensagens de commit seguindo o padrão Conventional Commits.

Leia `CLAUDE.md` e `docs/20260511/00-*` primeiro, depois abra um/uma issue para discutir qualquer questão estrutural antes do/da PR.