# GitHub — Postimi 3 nga 3 · Shënime Arkitekture (Diskutim në stilin ADR)

**Përdorimi si:** një Diskutim nën "Shfaq dhe trego" / "Arkitekturë", ose farë ADR për `docs/`.
**Fjalë kyçe:** arkitekturë, ADR, makinë gjendjeje vetëm-përpara, LLM lokal, Ollama, OCR, edge computing, CSP, header-a sigurie, pipeline të dhënash, inxhinieri kostosh, manifest SQLite, D1, R2, KV
**Hiperlinqe:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Pse ufolens.com është ndërtuar në këtë mënyrë

Shënime mbi tre vendimet që formësuan [ufolens.com](https://www.ufolens.com) (rindërtimi i kërkueshëm dhe shumëgjuhësh i [arkivit PURSUE UAP](https://www.war.gov/ufo)). Komentet / kundërshtimet janë të mirëpritura.

### 1. Pipeline-i është një makinë gjendjeje vetëm-përpara — me qëllim

Gjendjet: `zbuluar → shkarkuar → ocr_kryer → përkthyer → publikuar`. Një dokument lëviz vetëm përpara, dhe vetëm kur ka punë për të bërë. Përmbajtja e publikuar nuk ripërpunohet kurrë nëse një detektor delta nuk sheh që burimi ka ndryshuar realisht.

**Pse:** OCR + përkthimi janë operacionet e kushtueshme, dhe arkivi rritet me kalimin e kohës. Një pipeline që "riekzekuton gjithçka për siguri" ka kosto të pakufizuar. Bërja e tranzicioneve prapa të pamundura e bën një faturë të pakontrolluar të pamundur. Tavani i kostos është një veti e grafit të gjendjes, jo e vigjilencës së operatorit.

**Kostoja:** migrimet e skemës dhe ripërpunimi me qëllim janë qëllimisht të vështira. Një kompromis i pranueshëm.

### 2. OCR dhe përkthimi ekzekutohen në një LLM lokal, jo në një API cloud

OCR: motor me burim të hapur, rezervë Tesseract CLI. Përkthimi + NER: Gemma përmes Ollama, në një laptop Apple Silicon.

**Pse:** zero kosto marxhinale për dokument; e riprodhueshme (model fiks + prompts); dhe hapi i marrjes tashmë duhet të ekzekutohet nga një IP rezidenciale (burimi është pas Akamai Bot Manager — `curl` merr një 403), kështu që një laptop është gjithsesi në cikël.

**Kostoja:** cilësia e përkthimit është nën një model të fundit. Për një korpus referencë ku anglishtja origjinale është gjithmonë një klik larg, kjo është në rregull. Ne nuk pretendojmë se përkthimet janë autoritative.

### 3. Dy gjysmat ndajnë saktësisht një ndërfaqe: një pako e publikuar

Pipeline-i nuk shkruan kurrë drejtpërdrejt në bazën e të dhënave të prodhimit. Ai emeton `{ SQL, manifest asetesh, listë pastrimi cache }`. "Publikimi" = zbato atë pako përpara (dërgo SQL në DB-në SQL edge, sinkronizo asetet në ruajtjen e objekteve, pastro çelësat e emërtuar të cache-it).

**Pse:** ana lokale dhe ana edge mund të evoluojnë në mënyrë të pavarur; pakoja është e rishikueshme; dhe "vendosja e të dhënave" ka të njëjtën formë çdo herë. Worker-i është një aplikacion i vogël TypeScript/Hono — CSP i rreptë (pa `unsafe-inline`; JSON-LD i integruar është i fiksuar me sha256), negocim `Accept-Language` + vendi→gjuha, cache faqeje 30-ditor KV, cron ditor mirëmbajtjeje — dhe nuk ka nevojë kurrë të dijë se si u krijuan të dhënat.

**Kostoja:** një ndryshim skeme D1 prek dy skedarë (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Sigurim i lirë.

### Të panegociueshme të integruara në sjellje

- Nuk është i lidhur me qeverinë e SHBA-së; asnjë emblemë zyrtare.
- Redaktimet burimore ruhen, kurrë nuk kthehen mbrapsht.
- Video i atribuohet DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` në të gjithë sajtin — i indeksueshëm nga kërkimi, i çregjistruar nga scraping-u i AI.

Drejtpërdrejt: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
