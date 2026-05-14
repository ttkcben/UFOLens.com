# GitHub — Postitus 1/3 · Väljalaske / README teadaanne

**Kasutus:** GitHubi väljalaske kehana, kinnitatud aruteluna või repo README ülaosas.
**Märksõnad:** UAP, UFO, PURSUE arhiiv, salastatusest vabastatud dokumendid, avaandmed, täistekstiotsing, OCR, masintõlge, lokaalne LLM, Ollama, servatöötlus, avalik API, Hono, TypeScript, Python
**Hüperlingid:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — mitmekeelne, otsitav platvorm PURSUE UAP arhiivi jaoks

**Veebisait:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Lättearhiiv:** https://www.war.gov/ufo

`ufolens.com` avaldab uuesti USA sõjaministeeriumi **PURSUE** arhiivi salastatusest vabastatud UAP / UFO dokumentidest teadmusplatvormina: täistekstiotsing, masintõlge kogu korpuse ulatuses, kaardi ja ajajoonega tutvumine ning avalik JSON API. Lättedokumendid on USA föderaalvalitsuse teosed ja on USA-s üldkasutatavad ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). See projekt **ei ole seotud USA valitsusega**, ei kasuta ametlikke sümboleid ja ei tühista kunagi redigeerimisi.

### Arhitektuur

```
Lokaalne masin (Apple Silicon, elukoha IP)       Servavõrk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, ainult stdlib tuum)       worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (ainult edasi)    /{lang}/...   lehed
  OCR: avatud lähtekoodiga mootor (Tesseract CLI varuvariant) /api/v1/...   avalik API
  translate / NER: lokaalne LLM (Gemma Ollama kaudu)     /admin        operaatori konsool
  olek: SQLite manifest                              toetatud: serva SQL DB, objektimälu
        │                                              (lähte-PDF-id), KV vahemälu
        └── avaldab kimbu: SQL + varade manifest + vahemälu tühjendamise nimekiri ──┘
```

- **Null dokumendipõhist pilve-AI kulu.** OCR ja tõlkimine toimuvad lokaalselt; ainult edasiliikuv olekumasin (`discovered → downloaded → ocr_done → translated → published`) tagab, et ühtegi dokumenti ei töödelda uuesti, kui see pole muutunud.
- **Toru tuumal pole kolmandate osapoolte sõltuvusi** — parsimise / manifesti / delta moodulid töötavad ja testitakse puhtas Pythonis, kuhu pole midagi pip-iga installitud; OCR/tõlke etapid taanduvad sujuvalt, kui valikulised paketid puuduvad.
- **Servasait** rakendab rangeid turvapäiseid + CSP-d (ei ole `unsafe-inline`; inline JSON-LD on sha256-ga kinnitatud), keeleläbirääkimisi `Accept-Language` + riigi vastendamise kaudu, 30-päevast KV lehe vahemälu ja igapäevast korrastustööd.
- **Inkrementaalsed uuendused:** delta detektor võrdleb lähteindeksit ja edastab ainult muudatused tagasi torusse.

### Arendajatele

Avalik API aadressil https://www.ufolens.com/api/v1 tagastab dokumendid ja metaandmed JSON-vormingus. Anonüümne juurdepääs on kiiruspiiranguga; taotle võtit teadlase/arendaja tasemete jaoks. Vaata saidi API jaotist lõpp-punktide ja piirangute kohta.

### Olek

Kood on valmis; sait on kasutusele võetud aadressil https://www.ufolens.com. Tootmisandmebaas täidetakse võrguühenduseta toru käitamise ja kimbu edasi avaldamisega (`cli_publish run --remote`). Täielikud disainidokumendid asuvad kaustas `docs/20260511/`.

### Litsents / piirangud

- Lättedokumendid: USA föderaalvalitsuse teosed, USA-s üldkasutatavad.
- Selle platvormi enda kood: vaata `LICENSE`.
- Sait saadab päised `Tdm-Reservation: 1` ja `X-Robots-Tag: noai, noimageai` — otsingumootorite poolt indekseeritav, AI treeningust/kraapimisest on loobutud.
- Videomaterjal on omistatud DVIDS / AARO-le ja see projekt ei pretendeeri sellele.

Probleemid ja PR-id on teretulnud. Palun lugege enne struktuursete muudatuste avamist läbi `CLAUDE.md` ja `docs/20260511/00-*`.

