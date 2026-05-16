# GitHub — पोस्ट 1 ऑफ 3 · रिलीज़ / README घोषणा ब्लॉक

**उपयोग:** एक GitHub रिलीज़ बॉडी, एक पिन की गई चर्चा (Discussion), या रेपो README के शीर्ष पर।
**कीवर्ड:** UAP, UFO, PURSUE आर्काइव, डीक्लासिफाइड दस्तावेज़, ओपन डेटा, फुल-टेक्स्ट सर्च, OCR, मशीन ट्रांसलेशन, लोकल LLM, Ollama, एज कंप्यूटिंग, पब्लिक API, Hono, TypeScript, Python
**हाइपरलिंक्स:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP आर्काइव के लिए एक बहुभाषी, सर्च करने योग्य प्लेटफॉर्म

**लाइव:** https://www.ufolens.com  ·  **API:** https://www.ufolens.com/api/v1  ·  **सोर्स आर्काइव:** https://www.war.gov/ufo

`ufolens.com` अमेरिकी युद्ध विभाग (U.S. War Department) के डीक्लासिफाइड UAP / UFO रिकॉर्ड्स के **PURSUE** आर्काइव को एक नॉलेज प्लेटफॉर्म के रूप में पुनः प्रकाशित करता है: जिसमें फुल-टेक्स्ट सर्च, पूरे कॉर्पस में मशीन ट्रांसलेशन, मैप + टाइमलाइन एक्सप्लोरेशन, और एक पब्लिक JSON API शामिल हैं। सोर्स दस्तावेज़ अमेरिकी संघीय सरकार के कार्य हैं और अमेरिका के भीतर पब्लिक डोमेन ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)) में हैं। यह प्रोजेक्ट **अमेरिकी सरकार से संबद्ध नहीं है**, किसी आधिकारिक प्रतीक (insignia) का उपयोग नहीं करता है, और कभी भी रिडक्शन्स (redactions) को नहीं हटाता है।

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

- **प्रति-दस्तावेज़ शून्य क्लाउड-AI लागत।** OCR और ट्रांसलेशन स्थानीय रूप से (locally) चलते हैं; फॉरवर्ड-ओनली स्टेट मशीन (`discovered → downloaded → ocr_done → translated → published`) यह सुनिश्चित करती है कि जब तक दस्तावेज़ में बदलाव न हो, उसे दोबारा प्रोसेस नहीं किया जाएगा।
- **पाइपलाइन कोर में कोई थर्ड-पार्टी डिपेंडेंसी नहीं है** — पार्सिंग / मैनिफेस्ट / डेल्टा मॉड्यूल एक क्लीन Python पर चलते और टेस्ट होते हैं जिसमें कुछ भी pip-इंस्टॉल नहीं है; वैकल्पिक पैकेज न होने पर OCR/ट्रांसलेशन स्टेज ग्रेसफुली डिग्रेड (gracefully degrade) हो जाते हैं।
- **एज साइट (Edge site)** सख्त सुरक्षा हेडर + CSP (कोई `unsafe-inline` नहीं; इनलाइन JSON-LD sha256-पिन किया गया) लागू करती है, `Accept-Language` + कंट्री मैपिंग के माध्यम से भाषा नेगोशिएशन, 30-दिन का KV पेज कैश, और एक दैनिक हाउसकीपिंग क्रॉन (cron) का उपयोग करती है।
- **इन्क्रीमेंटल अपडेट:** एक डेल्टा डिटेक्टर सोर्स इंडेक्स का अंतर (diff) निकालता है और केवल बदलावों को वापस पाइपलाइन में भेजता है।

### डेवलपर्स के लिए

https://www.ufolens.com/api/v1 पर उपलब्ध पब्लिक API दस्तावेज़ों और मेटाडेटा को JSON के रूप में रिटर्न करता है। अनाम एक्सेस (Anonymous access) रेट-लिमिटेड है; रिसर्चर/डेवलपर टियर्स के लिए की (key) का अनुरोध करें। एंडपॉइंट्स और लिमिट्स के लिए साइट पर API सेक्शन देखें।

### स्टेटस

कोड पूरा हो चुका है; साइट https://www.ufolens.com. पर डिप्लॉय की गई है। प्रोडक्शन डेटाबेस को ऑफलाइन पाइपलाइन चलाकर और बंडल को फॉरवर्ड (`cli_publish run --remote`) पब्लिश करके पॉप्युलेट किया जाता है। पूर्ण डिज़ाइन दस्तावेज़ `docs/20260511/` में उपलब्ध हैं।

### लाइसेंस / सीमाएं

- सोर्स दस्तावेज़: अमेरिकी संघीय सरकार के कार्य, अमेरिका के भीतर पब्लिक डोमेन।
- इस प्लेटफॉर्म का अपना कोड: `LICENSE` देखें।
- साइट `Tdm-Reservation: 1` और `X-Robots-Tag: noai, noimageai` — भेजती है जो सर्च इंजनों द्वारा इंडेक्स किए जा सकते हैं, लेकिन AI ट्रेनिंग/स्क्रेपिंग से बाहर (opted out) रखा गया है।
- वीडियो फुटेज का श्रेय DVIDS / AARO को दिया गया है और इस प्रोजेक्ट द्वारा इसका दावा नहीं किया गया है।

इश्यूज (Issues) और PRs का स्वागत है। कृपया स्ट्रक्चरल बदलाव करने से पहले `CLAUDE.md` और `docs/20260511/00-*` पढ़ें।