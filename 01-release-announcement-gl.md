# GitHub — Publicación 1 de 3 · Bloque de anuncio de lanzamento / README

**Uso como:** corpo dun lanzamento de GitHub, unha discusión fixada ou a parte superior do README do repositorio.
**Palabras clave:** UAP, UFO, PURSUE archive, documentos desclasificados, datos abertos, busca de texto completo, OCR, tradución automática, LLM local, Ollama, edge computing, API pública, Hono, TypeScript, Python
**Hipervínculos:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — unha plataforma multilingüe e con capacidade de busca para o arquivo UAP de PURSUE

**En liña:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Arquivo fonte:** https://www.war.gov/ufo

`ufolens.com` volve publicar o arquivo **PURSUE** de rexistros desclasificados de UAP / UFO do Departamento de Guerra dos EE. UU. como unha plataforma de coñecemento: busca de texto completo, tradución automática en todo o corpus, exploración de mapa + cronoloxía e unha API JSON pública. Os documentos fonte son obras do goberno federal dos EE. UU. e dentro dos EE. UU. son de dominio público ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Este proxecto **non está afiliado ao goberno dos EE. UU.**, non utiliza insignias oficiais e nunca reverte as redaccións.

### Arquitectura

```
Máquina local (Apple Silicon, IP residencial)        Rede de borde (edge)
───────────────────────────────────────────           ───────────────────────────
pipeline/  (Python 3.10, núcleo só con stdlib)        worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (só de avance)     /{lang}/...   páxinas
  OCR: motor de código aberto (fallback Tesseract CLI)  /api/v1/...   API pública
  translate / NER: LLM local (Gemma vía Ollama)         /admin        consola de operador
  estado: manifesto SQLite                              respaldado por: BD SQL de borde,
        │                                              almacenamento de obxectos (PDF fonte),
        └── publica un paquete: SQL + manifesto de activos + lista de purga de caché ──┘
```

- **Custo cero por documento na nube de IA.** O OCR e a tradución execútanse localmente; a máquina de estados de só avance (`descuberto → descargado → ocr_feito → traducido → publicado`) garante que ningún documento se volva procesar a menos que cambiase.
- **O núcleo do pipeline non ten dependencias de terceiros** — os módulos de análise / manifesto / delta execútanse e próbanse nun Python limpo sen nada instalado con pip; as etapas de OCR/tradución degrádanse con elegancia cando os paquetes opcionais están ausentes.
- **O sitio de borde** aplica cabeceiras de seguridade estritas + CSP (sen `unsafe-inline`; JSON-LD en liña con pin sha256), negociación de idioma a través de `Accept-Language` + mapeo de países, unha caché de páxina KV de 30 días e un cron de mantemento diario.
- **Actualizacións incrementais:** un detector de deltas compara o índice fonte e só envía os cambios de volta ao pipeline.

### Para desenvolvedores

A API pública en https://www.ufolens.com/api/v1 devolve documentos e metadatos como JSON. O acceso anónimo está limitado por taxa; solicite unha chave para os niveis de investigador/desenvolvedor. Vexa a sección da API no sitio para os endpoints e límites.

### Estado

Código completo; sitio despregado en https://www.ufolens.com. A base de datos de produción pobóase executando o pipeline fóra de liña e publicando o paquete cara adiante (`cli_publish run --remote`). Os documentos de deseño completos atópanse en `docs/20260511/`.

### Licenza / límites

- Documentos fonte: obras do goberno federal dos EE. UU., dominio público dentro dos EE. UU.
- O código propio desta plataforma: vexa `LICENSE`.
- O sitio envía `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — indexable polos motores de busca, excluído do adestramento/scraping de IA.
- As gravacións de vídeo atribúense a DVIDS / AARO e non son reclamadas por este proxecto.

As Issues e PRs son benvidas. Por favor, lea `CLAUDE.md` e `docs/20260511/00-*` antes de abrir cambios estruturais.

