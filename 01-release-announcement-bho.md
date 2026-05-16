# GitHub — पोस्ट 1/3 · रिलीज / README घोषणा ब्लॉक

**उपयोग करीं:** एगो GitHub रिलीज बॉडी, एगो पिन कइल Discussion, भा रेपो README के ऊपर.
**कीवर्ड:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**हाइपरलिंक:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP आर्काइव खातिर एगो बहुभाषी, खोजे जोग प्लेटफॉर्म

**लाइव:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **स्रोत आर्काइव:** https://www.war.gov/ufo

`ufolens.com` अमेरिकी युद्ध विभाग के **PURSUE** डिक्लासिफाइड UAP / UFO रिकॉर्ड के आर्काइव के एगो ज्ञान प्लेटफॉर्म के रूप में दोबारा प्रकाशित करे ला: पूरा टेक्स्ट खोज, कॉर्पस भर में मशीन अनुवाद, नक्शा + टाइमलाइन खोज, आ एगो पब्लिक JSON API. स्रोत दस्तावेज अमेरिकी संघीय सरकार के काम हवें आ अमेरिका के भीतर पब्लिक डोमेन में बाड़न ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). ई परियोजना **अमेरिकी सरकार से संबद्ध नइखे**, कवनो आधिकारिक प्रतीक चिह्न के उपयोग ना करे ला, आ कवनो रिडैक्शन के कबो उलट ना करे ला.

### आर्किटेक्चर

```
Local machine (Apple Silicon, residential IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **प्रति-दस्तावेज जीरो क्लाउड-AI लागत.** OCR आ अनुवाद स्थानीय रूप से चले ला; खाली आगे बढ़े वाला स्टेट मशीन (`discovered → downloaded → ocr_done → translated → published`) ई गारंटी देवे ला कि कवनो दस्तावेज के तबले दोबारा संसाधित ना कइल जाई जबले ऊ बदल न जाई.
- **पाइपलाइन कोर में कवनो थर्ड-पार्टी निर्भरता नइखे** — पार्सिंग / मैनिफेस्ट / डेल्टा मॉड्यूल साफ Python पर चले लें आ टेस्ट होखे लें जवना में कवनो pip-स्थापित नइखे; OCR/अनुवाद चरण वैकल्पिक पैकेज के अनुपस्थिति में भी आसानी से काम करे लें.
- **एज साइट** सख्त सुरक्षा हेडर + CSP (कवनो `unsafe-inline` ना; इनलाइन JSON-LD sha256-पिन कइल गइल बा), `Accept-Language` + देश मैपिंग के माध्यम से भाषा बातचीत, 30 दिन के KV पेज कैश, आ एगो दैनिक हाउसकीपिंग क्रॉन लागू करे ला.
- **इंक्रीमेंटल अपडेट:** एगो डेल्टा डिटेक्टर स्रोत इंडेक्स के अंतर करे ला आ खाली बदलल चीज के पाइपलाइन में वापस फीड करे ला.

### डेवलपर लोग खातिर

https://www.ufolens.com/api/v1 पर पब्लिक API दस्तावेज आ मेटाडेटा के JSON के रूप में वापस करे ला. अनाम पहुँच के दर-सीमित कइल गइल बा; शोधकर्ता/डेवलपर टियर खातिर एगो कुंजी के अनुरोध करीं. एंडपॉइंट आ सीमा खातिर साइट पर API सेक्शन देखीं.

### स्थिति

कोड पूरा हो गइल बा; साइट https://www.ufolens.com पर डिप्लॉय हो गइल बा. प्रोडक्शन डेटाबेस ऑफलाइन पाइपलाइन चला के आ बंडल के आगे प्रकाशित क के भरल जाला (`cli_publish run --remote`). पूरा डिजाइन डॉक `docs/20260511/` में बाड़े.

### लाइसेंस / सीमा

- स्रोत दस्तावेज: अमेरिकी संघीय सरकार के काम, अमेरिका के भीतर पब्लिक डोमेन.
- ई प्लेटफॉर्म के आपन कोड: `LICENSE` देखीं.
- साइट `Tdm-Reservation: 1` आ `X-Robots-Tag: noai, noimageai` भेजे ला — सर्च इंजन द्वारा इंडेक्सेबल, AI प्रशिक्षण/स्क्रैपिंग से ऑप्ट आउट कइल गइल.
- वीडियो फुटेज DVIDS / AARO के बतावल गइल बा आ ई परियोजना द्वारा दावा नइखे कइल गइल.

मुद्दा आ PR के स्वागत बा. संरचनात्मक बदलाव खोले से पहिले `CLAUDE.md` आ `docs/20260511/00-*` पढ़ीं.

