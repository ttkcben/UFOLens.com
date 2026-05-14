# GitHub — Postarea 3 din 3 · Note de arhitectură (Discuție în stil ADR)

**Utilizare:** o Discuție sub „Show and tell” / „Architecture” sau ca bază pentru un ADR în `docs/`.
**Cuvinte cheie:** arhitectură, ADR, mașină de stări forward-only, LLM local, Ollama, OCR, edge computing, CSP, headere de securitate, pipeline de date, ingineria costurilor, manifest SQLite, D1, R2, KV
**Hyperlinkuri:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## De ce ufolens.com este construit așa cum este

Note despre cele trei decizii care au modelat [ufolens.com](https://www.ufolens.com) (reconstrucția căutabilă și multilingvă a [arhivei PURSUE UAP](https://www.war.gov/ufo)). Comentariile / opiniile contrare sunt binevenite.

### 1. Pipeline-ul este o mașină de stări forward-only — în mod intenționat

Stări: `discovered → downloaded → ocr_done → translated → published`. Un document avansează doar, și numai atunci când există ceva de făcut. Conținutul publicat nu este niciodată reprocesat, cu excepția cazului în care un detector de diferențe observă că sursa s-a schimbat efectiv.

**De ce:** OCR + traducerea sunt operațiunile costisitoare, iar arhiva crește în timp. Un pipeline care „rulează totul din nou pentru siguranță” are un cost nelimitat. Imposibilitatea tranzițiilor înapoi face imposibilă o factură scăpată de sub control. Plafonul de cost este o proprietate a grafului de stări, nu a vigilenței operatorului.

**Cost:** migrațiile de schemă și reprocesarea intenționată sunt deliberat incomode. Un compromis acceptabil.

### 2. OCR și traducerea rulează pe un LLM local, nu pe un API în cloud

OCR: motor open-source, cu fallback la Tesseract CLI. Traducere + NER: Gemma via Ollama, pe un laptop Apple Silicon.

**De ce:** cost marginal zero per document; reproductibil (model + prompturi fixe); și oricum, pasul de preluare trebuie să ruleze de pe un IP rezidențial (sursa este în spatele Akamai Bot Manager — `curl` primește un 403), deci un laptop este implicat în orice caz.

**Cost:** calitatea traducerii este sub cea a unui model de ultimă generație. Pentru un corpus de referință unde originalul în engleză este întotdeauna la un clic distanță, acest lucru este în regulă. Nu pretindem că traducerile sunt autoritare.

### 3. Cele două jumătăți partajează exact o interfață: un pachet publicat

Pipeline-ul nu scrie niciodată direct în baza de date de producție. El emite `{ SQL, manifest de resurse, listă de golire a cache-ului }`. „Publicarea” = aplicarea acelui pachet (push SQL la baza de date SQL edge, sincronizarea resurselor cu stocarea de obiecte, golirea cheilor de cache specificate).

**De ce:** partea locală și cea edge pot evolua independent; pachetul poate fi revizuit; și „implementarea datelor” are aceeași formă de fiecare dată. Worker-ul este o aplicație mică TypeScript/Hono — CSP strict (fără `unsafe-inline`; JSON-LD inline este fixat cu sha256), negociere `Accept-Language` + țară→limbă, cache KV pentru pagini de 30 de zile, cron zilnic de întreținere — și nu trebuie să știe niciodată cum au fost create datele.

**Cost:** o modificare a schemei D1 afectează două fișiere (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). O asigurare ieftină.

### Principii non-negociabile integrate în comportament

- Nu este afiliat cu guvernul S.U.A.; fără însemne oficiale.
- Cenzurările din sursă sunt păstrate, niciodată anulate.
- Materialele video sunt atribuite DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` pe întregul site — indexabil pentru căutare, cu opțiunea de a nu fi preluat de AI.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
