# GitHub — Støða 1 av 3 · Útgávufráboðan / README-blokkur

**Nýt sum:** ein GitHub Release-tekstur, ein festur Tjaktráður, ella ovast í README-fíluni.
**Leitorð:** UAP, UFO, PURSUE-skjalasavnið, frígivin skjøl, opin data, fulltekstaleitan, OCR, maskintýðing, lokalur LLM, Ollama, edge computing, alment API, Hono, TypeScript, Python
**Hyperleinkjur:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — ein fleirmálsligur, leitanligur pallur fyri PURSUE UAP-skjalasavnið

**Live:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **Kelduarkiv:** https://www.war.gov/ufo

`ufolens.com` endurútgevur **PURSUE**-skjalasavnið hjá U.S. War Department við frígivnum UAP / UFO-skjølum sum ein vitanarpall: fulltekstaleitan, maskintýðing gjøgnum alt savnið, kort- og tíðarlinjukanning, og eitt alment JSON API. Keldu skjølini eru verk hjá amerikansku alríkisstjórnini og eru innan fyri USA almenn ogn ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Henda verkætlanin er **ikki knýtt at amerikansku stjórnini**, nýtir eingi almenn merki, og vendir ongantíð strikaðum teksti um.

### Arkitekturur

```
Lokal maskina (Apple Silicon, privat IP)            Edge-netverk
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only kjarni)         worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (bert framá)      /{lang}/...   síður
  OCR: open-source motor (Tesseract CLI varaleið)    /api/v1/...   alment API
  translate / NER: lokalur LLM (Gemma via Ollama)      /admin        operatørkonsol
  state: SQLite manifest                             stuðlað av: edge SQL DB, objekt
        │                                              goymslu (keldu PDFs), KV cache
        └── útgevur ein bundil: SQL + asset manifest + cache-purge listi ──┘
```

- **Null kostnaður fyri ský-AI pr. skjal.** OCR og týðing koyra lokalt; einans-fram eftir gongandi støðumaskinan (`uppdagað → niðurtikið → ocr_liðugt → týtt → útgivið`) tryggjar, at einki skjal verður viðgjørt av nýggjum, uttan so at tað er broytt.
- **Kjarnin í rørskipanini hevur ongar triðjapartsbindingar** — parsing / manifest / delta-modulir koyra og verða royndir á einum reinum Python uttan nakað pip-installerað; OCR/týðingarstigini virka hóast valfríir pakkar mangla.
- **Edge-síðan** nýtir strongar trygdarheaderar + CSP (einki `unsafe-inline`; inline JSON-LD er sha256-fest), málsamráðing via `Accept-Language` + landakorting, ein 30-daga KV-síðucache, og eitt dagligt húsarhalds-cron.
- **Stigvísar dagføringar:** ein delta-detektor samanber kelduindeksið og gevur bara broytingar aftur í rørskipanina.

### Fyri mennarar

Alment API á https://www.ufolens.com/api/v1 skilar skjøl og metadata sum JSON. Anonym atgongd er avmarkað; bið um ein lykil fyri granskara/mennara-stig. Sí API-partin á síðuni fyri endapunkt og mark.

### Støða

Kodan er liðug; síðan er útrullað á https://www.ufolens.com. Framleiðslugrunnurin verður fyltur við at koyra offline-rørskipanina og útgeva bundilin framá (`cli_publish run --remote`). Fullfíggjaðir sniðgevingarskjøl liggja í `docs/20260511/`.

### Lisensur / avmarkingar

- Keldu skjøl: Verk hjá amerikansku alríkisstjórnini, almenn ogn innan fyri USA.
- Koda hjá hesum pallinum: sí `LICENSE`.
- Síðan sendir `Tdm-Reservation: 1` og `X-Robots-Tag: noai, noimageai` — kann indekserast av leitimaskinum, men er frávalt frá AI-venjing/skaving.
- Videoupptøkur eru tilskrivaðar DVIDS / AARO og verða ikki gjørdar krav uppá av hesi verkætlan.

Issues og PRs eru vælkomin. Vinaliga les `CLAUDE.md` og `docs/20260511/00-*` áðrenn tú letur upp fyri strukturellum broytingum.
