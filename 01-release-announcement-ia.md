# GitHub — Publicacion 1 de 3 · Annuncio de Release / bloco de README

**Usar como:** corpo de un Release de GitHub, un Discussion affixate, o al initio del README del repositorio.
**Parolas clave:** UAP, UFO, archivo PURSUE, documentos declassificate, datos aperte, recerca in texto complete, OCR, traduction automatic, LLM local, Ollama, computation al bordo, API public, Hono, TypeScript, Python
**Hyperligamines:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — un platteforma multilingue e perquisibile pro le archivo PURSUE UAP

**In directo:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Archivo fonte:** https://www.war.gov/ufo

`ufolens.com` republica le archivo **PURSUE** del Departimento de Guerra del S.U. de files declassificate de UAP / UFO como un platteforma de cognoscentia: recerca in texto complete, traduction automatic trans le corpus, exploration de mappa + chronologia, e un API public de JSON. Le documentos fonte son obras del governamento federal del S.U. e intra le S.U. son in le dominio public ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Iste projecto **non es affiliate con le governamento del S.U.**, non usa insignias official, e nunquam reverte redactiones.

### Architectura

```
Machina local (Apple Silicon, IP residential)         Rete al bordo
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, nucleo solo stdlib)         worker/  (TypeScript, Hono.js)
  recuperar → OCR → traducer → publicar (solo avante)  /{lang}/...   paginas
  OCR: motor de codice aperte (Tesseract CLI como reserva) /api/v1/...   API public
  traducer / NER: LLM local (Gemma via Ollama)         /admin        consola de operator
  stato: manifesto SQLite                            appoiate per: DB SQL al bordo,
        │                                              immagazinage de objectos (PDFs fonte),
        └── publica un pacchetto: SQL + manifesto de activos + lista de purga de cache ──┘
```

- **Costo zero de AI in le nube per documento.** OCR e traduction functiona localmente; le machina de statos a progression solmente (`discoperite → discargate → ocr_facte → traducite → publicate`) garanti que nulle documento es reprocessate a minus que illo ha cambiate.
- **Le nucleo del pipeline non ha dependentias de tertie partes** — le modulos de analyse / manifesto / delta functiona e testa sur un Python nette sin nihil installate via pip; le phases de OCR/traduction se degrada gratiosemente quando pacchettos optional es absente.
- **Le sito al bordo** applica stricte capites de securitate + CSP (non `unsafe-inline`; `inline JSON-LD` es affixate con `sha256`), negotiation de lingua via `Accept-Language` + mappamento de pais, un cache de pagina KV de 30 dies, e un cron de mantenentia diari.
- **Actualisationes incremental:** un detector de deltas compara le indice fonte e reinjecta solmente le cambios in le pipeline.

### Pro disveloppatores

Le API public a https://www.ufolens.com/api/v1 retorna documentos e metadatos como JSON. Le accesso anonyme es limitate in frequentia; solicita un clave pro nivellos de investigator/disveloppator. Vide le section del API sur le sito pro punctos de accesso e limites.

### Stato

Codice complete; sito displicate a https://www.ufolens.com. Le base de datos de production es populate per executar le pipeline offline e publicar le pacchetto avante (`cli_publish run --remote`). Le documentos de designo complete se trova in `docs/20260511/`.

### Licentia / frontieras

- Documentos fonte: obras del governamento federal del S.U., in le dominio public intra le S.U.
- Le codice proprie de iste platteforma: vide `LICENSE`.
- Le sito invia `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — indexabile per motores de recerca, optate foras del trainamento/scraping per AI.
- Le material video es attribuite a DVIDS / AARO e non es revendicate per iste projecto.

Problemas e PRs es benvenite. Per favor, lege `CLAUDE.md` e `docs/20260511/00-*` ante de aperir cambios structural.
