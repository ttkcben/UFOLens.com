# GitHub — Publicación 1 de 3 · Anunciu de llanzamientu / README

**Usu como:** cuerpu d'un llanzamientu de GitHub, un alderique afitáu o la parte cimera del README del repositoriu.
**Pallabres clave:** UAP, UFO, archivu PURSUE, documentos desclasificaos, datos abiertos, busca de testu completu, OCR, traducción automática, LLM local, Ollama, edge computing, API pública, Hono, TypeScript, Python
**Hiperenllaces:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — una plataforma multillingüe y con capacidá de busca pal archivu PURSUE UAP

**En direutu:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Archivu fonte:** https://www.war.gov/ufo

`ufolens.com` republica l'archivu **PURSUE** del Departamentu de Guerra de los EE.XX. de rexistros desclasificaos de UAP / UFO como una plataforma de conocencia: busca de testu completu, traducción automática en tol corpus, esploración per mapa y llinia de tiempu, y una API pública JSON. Los documentos fonte son obres del gobiernu federal de los EE.XX. y dientro de los EE.XX. son de dominiu públicu ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Esti proyeutu **nun ta afiliáu col gobiernu de los EE.XX.**, nun usa insinies oficiales y nunca revierte les censures.

### Arquiteutura

```
Máquina local (Apple Silicon, IP residencial)       Rede de borde (Edge network)
──────────────────────────────────────────          ──────────────────────────
pipeline/  (Python 3.10, nucleu solo stdlib)         worker/  (TypeScript, Hono.js)
  capturar → OCR → traducir → publicar (solo p'alantre) /{lang}/...   páxines
  OCR: motor de códigu abiertu (fallback Tesseract CLI) /api/v1/...   API pública
  traducir / NER: LLM local (Gemma vía Ollama)          /admin        consola d'operador
  estáu: manifiestu SQLite                           sofitáu por: BBDD SQL nel borde,
        │                                              almacenamientu d'oxetos (PDFs fonte),
        └── publica un paquete: SQL + manifiestu d'activos + llista de purga de caché ──┘         caché KV
```

- **Costu cero por documentu na nube d'IA.** L'OCR y la traducción execútense llocalmente; la máquina d'estaos de solo meyora (`descubiertu → descargáu → ocr_fechu → traducíu → publicáu`) garantiza que nengún documentu se reprocese sacantes que camudara.
- **El nucleu del pipeline nun tien dependencies de terceros** — los módulos de parseo / manifiestu / delta execútense y prueben nun Python llimpiu ensin nada instaláu con pip; les etapes d'OCR/traducción degraden con elegancia cuando falten paquetes opcionales.
- **El sitiu nel borde** aplica testeres de seguridá estrictes + CSP (ensin `unsafe-inline`; el JSON-LD en llinia ta afitáu con sha256), negociación d'idioma vía `Accept-Language` + mapeo de país, una caché de páxina en KV de 30 díes y un cron de caltenimientu diariu.
- **Actualizaciones incrementales:** un detector de deltes compara l'índiz fonte y solo unvia los cambeos de vuelta al pipeline.

### Pa desendolcadores

La API pública en https://www.ufolens.com/api/v1 devuelve documentos y metadatos como JSON. L'accesu anónimu ta llindáu por tasa; solicita una clave pa niveles d'investigador/desendolcador. Consulta la seición de la API nel sitiu pa ver los puntos finales y les llendes.

### Estáu

Códigu completu; sitiu desplegáu en https://www.ufolens.com. La base de datos de producción enllénase executando'l pipeline fuera de llinia y publicando'l paquete p'alantre (`cli_publish run --remote`). La documentación completa del diseñu ta en `docs/20260511/`.

### Llicencia / llendes

- Documentos fonte: obres del gobiernu federal de los EE.XX., de dominiu públicu dientro de los EE.XX.
- El códigu propiu d'esta plataforma: ver `LICENSE`.
- El sitiu unvia `Tdm-Reservation: 1` y `X-Robots-Tag: noai, noimageai` — indexable polos motores de busca, escluyíu del entrenamientu/scraping d'IA.
- El metraxe de videu atribúise a DVIDS / AARO y esti proyeutu nun reclama la so propiedá.

Los issues y PRs son bienveníos. Por favor, llee `CLAUDE.md` y `docs/20260511/00-*` enantes d'abrir cambeos estructurales.

