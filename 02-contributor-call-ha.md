# GitHub — Saƙo na 2 daga cikin 3 · Kiran gudunmawa / "batutuwa masu sauƙi na farko"

**Yi amfani da shi azaman:** Tattaunawa da aka maƙala ("Gudunmawa & batutuwa masu sauƙi na farko") ko gabatarwar CONTRIBUTING.md.
**Mahimman kalmomi:** buɗaɗɗen tushe, bayar da gudunmawa, batu mai sauƙi na farko, i18n, fassarar gida, OCR, Python, TypeScript, Vitest, pytest, samun dama, UAP, buɗaɗɗen bayanai
**Masu haɗin gwiwa:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Gudunmawa ga ufolens.com

[ufolens.com](https://www.ufolens.com) ya mai da [taskar PURSUE UAP](https://www.war.gov/ufo) ta Ma'aikatar Yaƙi ta Amurka zuwa dandali mai bincike, mai harsuna da yawa tare da [API na jama'a](https://www.ufolens.com/api/v1). Ya kasu kashi biyu — wani pipeline na Python na gida (`pipeline/`) da wani app na edge na TypeScript/Hono (`worker/`) — suna haɗuwa a wuri guda: wani haɗin SQL + kadarori da aka wallafa.

Ba kwa buƙatar kowane takaddun shaidar gajimare don bayar da gudunmawa. Sassan ainihin pipeline sun dogara ne kawai da stdlib kuma ana gudanar da gwaje-gwajen Worker akan ma'ajiyar ciki.

### Saitawa

```bash
# pipeline
python3 -m pytest pipeline/tests/          # ya kamata duk su zama kore, babu buƙatar shigar da pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Inda ake buƙatar taimako sosai

**i18n / fassarar gida** — `worker/src/i18n/ui-strings.json` shine asalin rubutun UI. Duba daga masu jin harshen asali ga kowane yare da ba na Turanci ba yana da matuƙar daraja: gano kurakuran fassarar inji, gyara matsalolin RTL/tsari, inganta matsalolin tattaunawar harshe.

**Ingancin OCR** — ingantaccen sarrafa tsoffin takardun da aka buga da tsohuwar na'ura kafin OCR; kayan aikin kimantawa da ke kwatanta injin budadden tushe da na Tesseract a kan wasu shafuka na samfuri.

**Samun dama** — duba shafukan da aka nuna (`worker/src/render/`) akan WCAG; CSP yana da tsauri (babu `unsafe-inline`), don haka dole ne mafita su yi aiki a cikin wannan iyakokin.

**Sauƙin amfani da API** — `worker/src/routes/` — raba shafuka, tacewa, bayanin OpenAPI, misalan abokan ciniki.

**Ƙarfin Pipeline** — ƙarin hanyoyin raguwa cikin sauƙi, ingantaccen rahoton ci gaba, matsalolin gano delta (`pipeline/lib/delta.py`).

**Takardu** — `docs/20260511/` (繁體中文; `00-*` shine jeri). Ana maraba da fassarar takardun zane zuwa Turanci.

### Dokokin Gida

- Duk hanyoyi dole su zama masu alaƙa — dole ne aikin ya zama mai ɗauka a cikin na'urori daban-daban. Babu cikakkun hanyoyin da aka rubuta kai tsaye.
- Kada a ƙara dogaro na pip a cikin babban sashin pipeline. Matakan zaɓi na iya amfani da fakitin zaɓi, kuma dole ne su ragu cikin sauƙi idan ba su nan.
- Kada a raunana injin jiha mai tafiya-gaba-kawai — wannan shine iyakar kuɗi.
- Kada a gabatar da alamun hukuma na gwamnatin Amurka, kuma kada a ƙara wani abu da ke cire gyare-gyaren da aka yi a asali.
- Canje-canjen tsarin D1 suna shafar fayiloli **biyu**: `pipeline/lib/manifest_schema.sql` da `db/schema.sql`.
- Gwajin tare da sabuwar lamba. Saƙonnin Conventional-commit.

Da farko karanta `CLAUDE.md` da `docs/20260511/00-*`, sannan ka buɗe batu don tattauna duk wani canji na tsari kafin PR.
