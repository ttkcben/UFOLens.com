# GitHub — Post 2 de 3 · Chamada para contribuidores / "boa primeira issues"

**Use como:** uma Discussão fixada ("Contributing & good first issues") ou como introdução do CONTRIBUTING.md.
**Keywords:** open source, contribuição, good first issue, i18n, localização, OCR, Python, TypeScript, Vitest, pytest, acessibilidade, UAP, dados abertos
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuindo para o ufolens.com

O [ufolens.com](https://www.ufolens.com) transforma o [PURSUE UAP archive](https://www.war.gov/ufo) do Departamento de Guerra dos EUA em uma plataforma multilíngue e pesquisável com um [ API público](https://www.ufolens.com/api/v1). Ele é composto por duas partes — um pipeline de ingestão Python local (`pipeline/`) e um app de edge TypeScript/Hono (`worker/`) — que se encontram em uma única interface: um pacote de SQL + assets publicado.

Você não precisa de credenciais de nuvem para contribuir. Os módulos core do pipeline utilizam apenas a stdlib e os testes do Worker são executados contra um armazenamento em memória.

### Setup

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Onde a ajuda é mais útil

**i18n / localização** — `worker/src/i18n/ui-strings.json` é a fonte das strings de UI. A revisão por falantes nativos de qualquer locale que não seja inglês é de alto valor: identificar traduções automáticas estranhas, corrigir RTL/issues de layout e melhorar casos extremos (edge cases) de negociação de idioma.

**Qualidade OCR** — melhoria no pré-processamento de scans de documentos antigos datilografados antes do OCR; estrutura de avaliação (evaluation harness) comparando a engine open source versus o fallback Tesseract em páginas de amostra.

**Acessibilidade** — auditar as páginas renderizadas (`worker/src/render/`) em relação ao WCAG; o CSP é rigoroso (sem `unsafe-inline`), portanto, as soluções devem funcionar dentro dessas limitações.

**Ergonomia API** — `worker/src/routes/` — paginação, filtragem, descrição OpenAPI, clientes de exemplo.

**Robustez do pipeline** — caminhos de degradação suave (graceful degradation) mais eficientes, melhores relatórios de progresso, casos extremos de detecção de delta (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` é o índice). Traduções dos documentos de design para o inglês são bem-vindas.

### Regras básicas

- Todos os caminhos relativos — ao projeto devem ser portáveis entre máquinas. Não utilize caminhos absolutos hardcoded.
- Não adicione dependências de pip a um módulo *core* do pipeline. Estágios opcionais podem usar pacotes opcionais e devem degradar suavemente sem eles.
- Não fragilize a máquina de estados apenas para frente (forward-only) —, pois esse é o teto de custo.
- Não introduza insígnias oficiais do governo dos EUA e não adicione nada que reverta as rasuras (redactions) da fonte.
- Alterações no esquema do D1 afetam **dois** arquivos: `pipeline/lib/manifest_schema.sql` e `db/schema.sql`.
- Inclua testes com novos códigos. Mensagens de commit seguindo o padrão Conventional Commits.

Leia `CLAUDE.md` e `docs/20260511/00-*` primeiro, depois abra um issue para discutir qualquer questão estrutural antes do PR.