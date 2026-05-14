# GitHub — Post 3 di 3 · Noti di architittura (Discussioni stili-ADR)

**Usu comu:** na Discussioni sutta "Ammustra e cunta" / "Architittura", o comu simenza pi `docs/` ADR.
**Paroli chiavi:** architittura, ADR, màchina a stati sulu-avanti, LLM lucali, Ollama, OCR, edge computing, CSP, header di sicurizza, pipeline di dati, ngignirìa dî costi, manifestu SQLite, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Picchì ufolens.com è custruitu com'è

Noti supra li tri dicisioni ca hannu furmatu [ufolens.com](https://www.ufolens.com) (la ricustruzzioni ricircàbbili e multilingui di l'[archiviu PURSUE UAP](https://www.war.gov/ufo)). Cummenti / crìtichi sunnu benvinuti.

### 1. La pipeline è na màchina a stati sulu-avanti — apposta

Stati: `scupertu → scaricatu → ocr_fattu → traduciutu → pubblicatu`. Un ducumentu si movi sulu n'avanti, e sulu quannu c'è travagghiu di fari. Lu cuntinutu pubblicatu nun veni mai ri-elabburatu a menu ca un rilevaturi di delta nun vidi ca la surgenti canciò pi daveru.

**Picchì:** L'OCR + la traduzzioni sunnu l'opirazzioni cchiù custusi, e l'archiviu crisci cû tempu. Na pipeline ca "rifà tuttu pi sicurizza" havi un costu senza limiti. Rènniri mpussìbbili li transizzioni n'arrè fa mpussìbbili na spisa fora cuntrollu. Lu tettu massimu di costu è na pruprietà dû grafu di statu, nun dâ viggilanza di l'upiraturi.

**Costu:** li migrazzioni di schema e la ri-elabburazzioni apposta sunnu volutamenti scomudi. Un cumprumissu accittàbbili.

### 2. L'OCR e la traduzzioni funzionanu supra un LLM lucali, nun supra n'API cloud

OCR: muturi open-source, fallback Tesseract CLI. Traduzzioni + NER: Gemma via Ollama, supra un laptop Apple Silicon.

**Picchì:** costu marginali zero p'ugni ducumentu; riproducìbbili (mudellu fissu + prompts); e la fasi di fetch havi già a funziunari di n'IP risidinziali (la surgenti è d'arrè a Akamai Bot Manager — `curl` pigghia un 403), quinni un laptop è cumunqui ntô ciclu.

**Costu:** la qualità dâ traduzzioni è cchiù vascia di un mudellu di fruntiera. Pi un corpus di riferimentu unni l'origginali n anglisi è sempri a un clic di distanza, va beni. Nun affirmamu ca li traduzzioni sunnu auturitarii.

### 3. Li dui menzi spàrtinu esattamenti n'interfaccia: un pacchettu pubblicatu

La pipeline nun scrivi mai direttamenti ntô databasi di produzzioni. Emetti un `{ SQL, manifestu d'asset, lista purga-cache }`. "Pubblicari" = applicari ssu pacchettu n'avanti (pushari lu SQL ô DB SQL edge, sincrunizzari l'assets ô storage d'uggetti, purgari li chiavi di cache numinati).

**Picchì:** la parti lucali e la parti edge ponnu evòlviri nnipinnentimenti; lu pacchettu è rivisiunàbbili; e "distribuiri dati" havi la stissa forma ogni vota. Lu Worker è na nica app TypeScript/Hono — CSP rigurusu (nuddu `unsafe-inline`; lu JSON-LD inline è fissatu cu sha256), niguzziazzioni `Accept-Language` + paisi→lingua, cache di pàggina KV di 30 jorna, cron di manutinzioni jurnalera — e nun havi mai bisognu di sapiri comu foru fatti li dati.

**Costu:** un canciamentu di schema D1 tucca dui file (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Assicurazzioni a bon prezzu.

### Nun-niguzziàbbili ncurpurati ntô cumpurtamentu

- Nun affiliatu cû guvernu dî Stati Uniti; nudda nsigna ufficiali.
- Li redazzioni dâ surgenti sunnu prisirvati, mai annullati.
- Vìdiu attribuitu a DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` pi tuttu lu situ — indicizzàbbili pi ricerca, disattivatu pi scraping di l'AI.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

