# GitHub — Publicacion 1 de 3 · Blòt d'anóncia de version / README

**Utilizar coma:** un còrs de GitHub Release, una discussion apngada, o lo naut del README del depaus.
**Mots clau:** UAP, UFO, archius PURSUE, documents desclassificats, donadas obèrtas, cèrca en tèxte integral, OCR, traduccion automatica, LLM local, Ollama, edge computing, API publica, Hono, TypeScript, Python
**Iperligams:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — una plataforma multilingüa e cercabla per las archius UAP PURSUE

**En linha:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archius font:** https://www.war.gov/ufo

`ufolens.com` torna publicar las archius **PURSUE** del Departament de la Guèrra dels EUA sus los documents desclassificats UAP / UFO coma una plataforma de coneissenças: cèrca en tèxte integral, traduccion automatica a travèrs del corpus, exploracion per mapa e cronologia, e una API JSON publica. Los documents font son d'òbras del govèrn federal dels EUA e, als EUA, son del domeni public ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Aqueste projècte es **pas afiliat al govèrn dels EUA**, utiliza pas d'insignes oficials e jamai anulla las redaccions.

### Arquitectura

```
Maquinari local (Apple Silicon, IP residenciala)    Ret Edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, còr stdlib-only)            worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (avançament unic) /{lang}/...   paginas
  OCR: motor open-source (recors a la CLI Tesseract)  /api/v1/...   API publica
  translate / NER: LLM local (Gemma via Ollama)       /admin        consòla operator
  estat: manifèst SQLite                            sostengut per: BD SQL edge, estocatge
        │                                              d'objèctes (PDFs font), cache KV
        └── publica un paquet: SQL + manifèst d'actius + lista de purga del cache ──┘
```

- **Còst zèro per document per l'IA en nívol.** L'OCR e la traduccion s'executan localament; la maquina d'estats d'avançament unic (`discovered → downloaded → ocr_done → translated → published`) garantís qu'un document es pas tornat tractar levat s'a cambiat.
- **Lo còr del pipeline a pas de dependéncias de tèrces** — los moduls d'analisi / manifèst / delta s'executan e se testan sus un Python net sens res d'installat amb pip; las estapas d'OCR/traduccion se degradan elegantament quand los paquets opcionals son absents.
- **Lo site Edge** aplica d'entèstas de seguretat estrictas + CSP (pas de `unsafe-inline`; lo JSON-LD en linha es fixat amb sha256), negociacion de lenga via `Accept-Language` + mapatge de país, un cache de pagina KV de 30 jorns e un cron de mantenença jornalièr.
- **Misas a jorn incrementalas:** un detector de deltas compara l'indèx font e provesís solament los cambiaments al pipeline.

### Pels desvolopaires

L'API publica a https://www.ufolens.com/api/v1 retorna de documents e de metadonadas en JSON. L'accès anonim es limitat en debit; demandatz una clau per de nivèls cercaire/desvolopaire. Vejatz la seccion API sul site per los punts d'accès e las limitas.

### Estat

Còdi complet; site desplegat a https://www.ufolens.com. La basa de donadas de produccion es poblada en executant lo pipeline fòra linha e en publicant lo paquet cap avant (`cli_publish run --remote`). Los documents de concepcion complets se tròban dins `docs/20260511/`.

### Licéncia / Limitas

- Documents font: òbras del govèrn federal dels EUA, domeni public als EUA.
- Lo còdi pròpri d'aquesta plataforma: veire `LICENSE`.
- Lo site envia `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — indexable pels motors de cèrca, desactivat per l'entrainament/rasclatge de l'IA.
- Las sequéncias vidèo son atribuidas a DVIDS / AARO e son pas revendicadas per aqueste projècte.

Issues e PRs son benvenguts. Mercés de legir `CLAUDE.md` e `docs/20260511/00-*` abans d'obrir de cambiaments estructurals.
