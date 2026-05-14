# GitHub — Publicació 1 de 3 · Bloc d'anunci de Llançament / README

**Ús:** com a cos d'un llançament de GitHub, una Discussió fixada, o la part superior del README del repositori.
**Paraules clau:** UAP, UFO, arxiu PURSUE, documents desclassificats, dades obertes, cerca de text complet, OCR, traducció automàtica, LLM local, Ollama, computació edge, API pública, Hono, TypeScript, Python
**Hipervincles:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — una plataforma multilingüe i cercable per a l'arxiu PURSUE UAP

**En viu:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Arxiu font:** https://www.war.gov/ufo

`ufolens.com` torna a publicar l'arxiu **PURSUE** del Departament de Guerra dels EUA de registres desclassificats d'UAP / UFO com a plataforma de coneixement: cerca de text complet, traducció automàtica en tot el corpus, exploració de mapa i cronologia, i una API JSON pública. Els documents font són obres del govern federal dels EUA i, dins dels EUA, són de domini públic ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Este projecte **no està afiliat amb el govern dels EUA**, no utilitza cap insígnia oficial i mai reverteix les redaccions.

### Arquitectura

```
Màquina local (Apple Silicon, IP residencial)        Xarxa edge
──────────────────────────────────────────           ──────────────────────────
pipeline/  (Python 3.10, nucli només amb stdlib)      worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (només cap avant)   /{lang}/...   pàgines
  OCR: motor de codi obert (alternativa Tesseract CLI)    /api/v1/...   API pública
  translate / NER: LLM local (Gemma a través d'Ollama)       /admin        consola d'operador
  estat: manifest SQLite                                recolzat per: BD SQL edge, emmagatzematge
        │                                                 d'objectes (PDFs font), caché KV
        └── publica un paquet: SQL + manifest d'actius + llista de purga de caché ──┘
```

- **Cost zero d'IA en el núvol per document.** L'OCR i la traducció s'executen localment; la màquina d'estats de només avanç (`discovered → downloaded → ocr_done → translated → published`) garanteix que cap document es reprocessa a menys que haja canviat.
- **El nucli del pipeline no té dependències de tercers** — els mòduls d'anàlisi / manifest / delta s'executen i proven en un Python net sense res instal·lat amb pip; les etapes d'OCR/traducció es degraden amb gràcia quan els paquets opcionals no hi són.
- **El lloc edge** aplica capçaleres de seguretat estrictes + CSP (sense `unsafe-inline`; el JSON-LD en línia està fixat amb sha256), negociació d'idioma mitjançant `Accept-Language` + mapeig de països, una caché de pàgina KV de 30 dies i un cron de manteniment diari.
- **Actualitzacions incrementals:** un detector de deltes compara l'índex font i només envia els canvis de nou al pipeline.

### Per a desenvolupadors

L'API pública en https://www.ufolens.com/api/v1 retorna documents i metadades com a JSON. L'accés anònim té un límit de taxa; sol·liciteu una clau per als nivells d'investigador/desenvolupador. Consulteu la secció de l'API en el lloc per a vore els endpoints i els límits.

### Estat

Codi completat; lloc desplegat en https://www.ufolens.com. La base de dades de producció s'ompli executant el pipeline fora de línia i publicant el paquet cap avant (`cli_publish run --remote`). Els documents de disseny complets es troben en `docs/20260511/`.

### Llicència / límits

- Documents font: obres del govern federal dels EUA, de domini públic dins dels EUA.
- El codi propi d'esta plataforma: vegeu `LICENSE`.
- El lloc envia `Tdm-Reservation: 1` i `X-Robots-Tag: noai, noimageai` — indexable pels motors de cerca, exclòs de l'entrenament/scraping d'IA.
- El metratge de vídeo s'atribueix a DVIDS / AARO i no és reclamat per este projecte.

Els issues i PRs són benvinguts. Per favor, llegiu `CLAUDE.md` i `docs/20260511/00-*` abans d'obrir canvis estructurals.

