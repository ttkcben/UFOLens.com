# GitHub – Innlegg 3 av 3 · Arkitekturnotater (Diskusjon i ADR-stil)

**Brukes som:** en diskusjon under "Vis og fortell" / "Arkitektur", eller som utgangspunkt for en ADR i `docs/`.
**Nøkkelord:** arkitektur, ADR, enveis-tilstandsmaskin, lokal LLM, Ollama, OCR, edge computing, CSP, sikkerhetshoder, datapipeline, kostnadsstyring, SQLite-manifest, D1, R2, KV
**Hyperlenker:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Hvorfor ufolens.com er bygget som det er

Notater om de tre avgjørelsene som formet [ufolens.com](https://www.ufolens.com) (den søkbare, flerspråklige gjenoppbyggingen av [PURSUE UAP-arkivet](https://www.war.gov/ufo)). Kommentarer / motforestillinger er velkomne.

### 1. Pipelinen er en enveis-tilstandsmaskin – med vilje

Tilstander: `oppdaget → nedlastet → ocr_ferdig → oversatt → publisert`. Et dokument beveger seg kun fremover, og bare når det er arbeid å gjøre. Publisert innhold blir aldri behandlet på nytt med mindre en deltadetektor ser at kilden faktisk har endret seg.

**Hvorfor:** OCR + oversettelse er de kostbare operasjonene, og arkivet vokser over tid. En pipeline som "kjører alt på nytt for å være sikker" har ubegrenset kostnad. Å gjøre tilbakegående overganger umulig gjør en løpsk regning umulig. Kostnadstaket er en egenskap ved tilstandsgrafen, ikke av operatørens årvåkenhet.

**Kostnad:** skjemamigreringer og bevisst reprosessering er med vilje gjort tungvint. En akseptabel avveining.

### 2. OCR og oversettelse kjører på en lokal LLM, ikke et sky-API

OCR: open source-motor, Tesseract CLI som fallback. Oversettelse + NER: Gemma via Ollama, på en Apple Silicon-laptop.

**Hvorfor:** null marginalkostnad per dokument; reproduserbarhet (fast modell + prompts); og hentesteget må allerede kjøre fra en privat IP-adresse (kilden ligger bak Akamai Bot Manager — `curl` får en 403), så en laptop er uansett involvert.

**Kostnad:** oversettelseskvaliteten er under den til en «frontier»-modell. For et referansekorpus der den originale engelske teksten alltid er ett klikk unna, er det greit. Vi hevder ikke at oversettelsene er autoritative.

### 3. De to halvdelene deler nøyaktig ett grensesnitt: en publisert pakke

Pipelinen skriver aldri direkte til produksjonsdatabasen. Den produserer `{ SQL, ressursmanifest, liste for tømming av cache }`. "Publisering" = anvende den pakken fremover (push SQL til edge SQL DB, synkroniser ressurser til object storage, tøm de navngitte cache-nøklene).

**Hvorfor:** den lokale siden og edge-siden kan utvikle seg uavhengig; pakken kan gjennomgås; og "datadeployering" har samme form hver gang. Worker-en er en liten TypeScript/Hono-app — streng CSP (ingen `unsafe-inline`; inline JSON-LD er sha256-festet), `Accept-Language` + land→språk-forhandling, 30-dagers KV-sidebuffer, daglig vedlikeholds-cronjobb — og den trenger aldri å vite hvordan dataene ble laget.

**Kostnad:** en D1-skjemaendring berører to filer (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Billig forsikring.

### Ikke-forhandlebare prinsipper integrert i atferden

- Ikke tilknyttet den amerikanske regjeringen; ingen offisielle insignier.
- Kildesladding bevares, aldri reversert.
- Video tilskrevet DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` for hele nettstedet — søkeindekserbar, reservert mot AI-skraping.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
