# GitHub — Publicació 1 de 3 · Bloc d'anunci de llançament / README

**Ús:** com a cos d'un llançament de GitHub, una discussió fixada o a la part superior del README del repositori.
**Paraules clau:** UAP, UFO, arxiu PURSUE, documents desclassificats, dades obertes, cerca de text complet, OCR, traducció automàtica, LLM local, Ollama, edge computing, API pública, Hono, TypeScript, Python
**Enllaços:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — una plataforma multilingüe i cercable per a l'arxiu PURSUE UAP

**En directe:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Arxiu font:** https://www.war.gov/ufo

`ufolens.com` torna a publicar l'arxiu **PURSUE** del Departament de Guerra dels EUA de registres desclassificats d'UAP / UFO com a plataforma de coneixement: cerca de text complet, traducció automàtica a tot el corpus, exploració de mapes + línies de temps i una API pública JSON. Els documents font són obres del govern federal dels EUA i, als EUA, són de domini públic ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Aquest projecte **no està afiliat al govern dels EUA**, no utilitza cap insígnia oficial i no reverteix mai les redaccions.

### Arquitectura

```
Màquina local (Apple Silicon, IP residencial)        Xarxa edge
───────────────────────────────────────────         ─────────────────────────
pipeline/  (Python 3.10, nucli només stdlib)         worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (només d'avanç)   /{lang}/...   pàgines
  OCR: motor de codi obert (Tesseract CLI com a fallback) /api/v1/...   API pública
  translate / NER: LLM local (Gemma via Ollama)       /admin        consola d'operador
  estat: manifest SQLite                              recolzat per: BD SQL edge, emmagatzematge
        │                                               d'objectes (PDFs font), cau KV
        └── publica un paquet: SQL + manifest d'actius + llista de purga de cau ──┘
```

- **Cost zero d'IA al núvol per document.** L'OCR i la traducció s'executen localment; la màquina d'estats només d'avanç (`descobert → descarregat → ocr_fet → traduït → publicat`) garanteix que cap document es reprocessi a menys que hagi canviat.
- **El nucli del pipeline no té dependències de tercers** — els mòduls d'anàlisi / manifest / delta s'executen i proven en un Python net sense res instal·lat amb pip; les etapes d'OCR/traducció es degraden amb elegància quan els paquets opcionals no hi són.
- **El lloc edge** aplica capçaleres de seguretat estrictes + CSP (sense `unsafe-inline`; el JSON-LD incrustat està fixat amb sha256), negociació d'idioma mitjançant `Accept-Language` + mapatge de països, una cau de pàgines a KV de 30 dies i un cron de manteniment diari.
- **Actualitzacions incrementals:** un detector de deltes compara l'índex font i només envia els canvis de nou al pipeline.

### Per a desenvolupadors

L'API pública a https://www.ufolens.com/api/v1 retorna documents i metadades com a JSON. L'accés anònim té un límit de taxa; sol·liciteu una clau per a nivells d'investigador/desenvolupador. Consulteu la secció de l'API al lloc per veure els punts d'accés i els límits.

### Estat

Codi complet; lloc desplegat a https://www.ufolens.com. La base de dades de producció s'omple executant el pipeline fora de línia i publicant el paquet (`cli_publish run --remote`). Els documents de disseny complets es troben a `docs/20260511/`.

### Llicència / límits

- Documents font: Obres del govern federal dels EUA, domini públic dins dels EUA.
- El codi propi d'aquesta plataforma: vegeu `LICENSE`.
- El lloc envia `Tdm-Reservation: 1` i `X-Robots-Tag: noai, noimageai` — indexable pels motors de cerca, exclòs de l'entrenament/extracció d'IA.
- Les gravacions de vídeo s'atribueixen a DVIDS / AARO i no són reclamades per aquest projecte.

Els issues i PRs són benvinguts. Si us plau, llegiu `CLAUDE.md` i `docs/20260511/00-*` abans d'obrir canvis estructurals.

