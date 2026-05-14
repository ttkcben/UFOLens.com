# GitHub — Publicassion 1 ëd 3 · Blòch ëd publicassion Release / README

**Dovré com:** còrp ëd na Release GitHub, na Discussion fërmà, o an testa dël README dël deposit.
**Paròle ciav:** UAP, UFO, archiv PURSUE, document declassificà, dàit duvert, arserca testual completa, OCR, tradussion automàtica, LLM local, Ollama, edge computing, API pùblica, Hono, TypeScript, Python
**Anliure:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — na piataforma multilingua e sërcàbil për l'archiv PURSUE UAP

**An diresta:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Archiv sorgiss:** https://www.war.gov/ufo

`ufolens.com` a ripublica l'archiv **PURSUE** dël Dipartiment dla Guèra djë Stat Unì con document declassificà UAP / UFO 'me na piataforma ëd conossensa: arserca testual completa, tradussion automàtica an tut l'archiv, esplorassion su mapa + linia dël temp, e n'API pùblica JSON. Ij document sorgiss a son euvre dël goern federal djë Stat Unì e, andrinta jë Stat Unì, a son ëd domini pùblich ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Sto proget a l'é **pa afilià con ël goern djë Stat Unì**, a deuvra gnun-a ansigna ofissial, e a n'anula mai le redassion.

### Architetura

```
Màchina local (Apple Silicon, IP residensial)     Rèj Edge
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, cheur mach stdlib)        worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (mach anans)    /{lang}/...   pàgine
  OCR: motor open-source (Tesseract CLI 'me riserva)  /api/v1/...   API pùblica
  translate / NER: LLM local (Gemma via Ollama)      /admin        coànsola operator
  stat: manifest SQLite                              apogià da: DB SQL edge, memòria
        │                                              d'oget (PDF sorgiss), cache KV
        └── a pùblica un pachet: SQL + manifest asset + lista purge cache ──┘
```

- **Zero cost ëd cloud-AI për document.** OCR e tradussion a giro localment; la màchina a stat mach anans (`dëscoatà → dëscarià → ocr_fait → traducì → publicà`) a garantiss che gnun document a sia rielaborà, gavà ch'a sia cambià.
- **Ël cheur dla pipeline a l'ha gnun-a dipendensa da ters** — ij mòduj ëd parsing / manifest / delta a giro e a son testà su un Python polid, sensa gnente instalà con pip; jë stadi d'OCR/tradussion a degrado con grassia quand che ij pachet opsionaj a-i son nen.
- **Ël sìt Edge** a aplica d'antestassion ëd sigurëssa severe + CSP (gnun `unsafe-inline`; JSON-LD an linia fissà con sha256), negossiassion dla lenga via `Accept-Language` + corëspondensa pais, na cache ëd pàgina KV ëd 30 dì, e un cron ëd polissìa giornalié.
- **Agiornament incrementaj:** un detetor ëd delta a contròla le diferense ant l'ìndes sorgiss e a forniss mach ij cambiament a la pipeline.

### Për jë svilupador

L'API pùblica a https://www.ufolens.com/api/v1 a forniss document e metadàit 'me JSON. L'acess anònim a l'é limità an frequensa; ciamé na ciav për ij livèj da arsercador/svilupador. Vëdde la session API sël sìt për j'endpoint e ij lìmit.

### Stat

Còdes complet; sìt an linia su https://www.ufolens.com. La base ëd dàit ëd produssion a l'é popolà an fasend giré la pipeline fòra linia e an publicand ël pachet anans (`cli_publish run --remote`). Ij document complet ëd proget a son an `docs/20260511/`.

### Licensa / confin

- Document sorgiss: euvre dël goern federal djë Stat Unì, an domini pùblich andrinta jë Stat Unì.
- Ël còdes pròpi ëd sa piataforma: vëdde `LICENSE`.
- Ël sìt a manda `Tdm-Reservation: 1` e `X-Robots-Tag: noai, noimageai` — indicisàbil da ij motor d'arserca, gavà da la racòlta dàit për l'IA.
- Le filmà video a son atribuìe a DVIDS / AARO e a son pa rivendicà da sto proget.

Problema e PR a son bin accetà. Për piasì, lese `CLAUDE.md` e `docs/20260511/00-*` prima ëd duverté modìfiche struturej.

