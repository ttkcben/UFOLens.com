# GitHub – Innlegg 3 av 3 · Arkitekturnotat (ADR-stil diskusjon)

**Bruk som:** ein diskusjon under «Vis og fortel» / «Arkitektur», eller `docs/` ADR-frø.
**Nøkkelord:** arkitektur, ADR, berre-framover-tilstandsmaskin, lokal LLM, Ollama, OCR, edge computing, CSP, tryggleikshovud, datastrøym, kostnadsstyring, SQLite manifest, D1, R2, KV
**Hyperlenker:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Kvifor ufolens.com er bygd slik det er

Notat om dei tre avgjerdene som forma [ufolens.com](https://www.ufolens.com) (den søkbare, fleirspråklege gjenoppbygginga av [PURSUE UAP-arkivet](https://www.war.gov/ufo)). Kommentarar / motstand er velkomne.

### 1. Røyrleidninga er ein berre-framover-tilstandsmaskin – med vilje

Tilstandar: `oppdaga → lasta ned → ocr_ferdig → omsett → publisert`. Eit dokument flyttar seg berre framover, og berre når det er arbeid å gjere. Publisert innhald blir aldri behandla på nytt med mindre ein deltadetektor ser at kjelda faktisk har endra seg.

**Kvifor:** OCR + omsetjing er dei dyre operasjonane, og arkivet veks over tid. Ei røyrleidning som «køyrer alt på nytt for å vere sikker» har ubegrensa kostnad. Å gjere bakoverovergangar umogleg gjer ein løpsk rekning umogleg. Kostnadstaket er ein eigenskap ved tilstandsgrafen, ikkje operatørvaktsemd.

**Kostnad:** skjemamigreringar og vilja re-prosessering er med vilje vanskeleg. Akseptabel avveging.

### 2. OCR og omsetjing køyrer på ein lokal LLM, ikkje ein sky-API

OCR: open-source motor, Tesseract CLI fallback. Omsetjing + NER: Gemma via Ollama, på ein Apple Silicon-laptop.

**Kvifor:** null marginalkostnad per dokument; reproduserbart (fast modell + prompts); og hentesteget må uansett køyre frå ein privat IP (kjelda er bak Akamai Bot Manager – `curl` får ein 403), så ein laptop er uansett i loopen.

**Kostnad:** omsetjingskvaliteten er under ein toppmodell. For ein referansekorpus der den originale engelsken alltid er eitt klikk unna, er det greitt. Vi hevdar ikkje at omsetjingane er autoritative.

### 3. Dei to halvdelane deler nøyaktig eitt grensesnitt: ein publisert pakke

Røyrleidninga skriv aldri direkte til produksjonsdatabasen. Ho sender ut `{ SQL, ressursmanifest, cache-tømmeliste }`. «Publisering» = bruk den pakken framover (push SQL til edge SQL DB, synkroniser ressursar til object storage, tøm dei namngjevne cache-nøklane).

**Kvifor:** den lokale sida og edge-sida kan utvikle seg uavhengig; pakken er etterprøvbar; og «deployer data» har same form kvar gong. Worker-en er ein liten TypeScript/Hono-app – streng CSP (ingen `unsafe-inline`; inline JSON-LD er sha256-festa), `Accept-Language` + land→språk-forhandling, 30-dagars KV-sidecache, dagleg vedlikehalds-cron – og han treng aldri å vite korleis dataa vart laga.

**Kostnad:** ei D1-skjemaendring rører ved to filer (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Billig forsikring.

### Ikkje-forhandlingsbar åtferd bakt inn

- Ikkje tilknytt den amerikanske regjeringa; ingen offisielle emblem.
- Kjelde-sladdingar er bevarte, aldri reverserte.
- Video tilskriven DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` over heile sida – søke-indekserbar, reservert mot AI-skraping.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
