# GitHub — पोस्ट 2 ऑफ 3 · योगदानकर्ता कॉल / "गुड फर्स्ट issues"

**उपयोग करें:** एक पिन की गई चर्चा ("Contributing & good first issues") या CONTRIBUTING.md परिचय के रूप में।
**कीवर्ड:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**हाइपरलिंक्स:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com में योगदान देना

[ufolens.com](https://www.ufolens.com) अमेरिकी युद्ध विभाग (U.S. War Department) के [PURSUE UAP आर्काइव](https://www.war.gov/ufo) को एक सर्च करने योग्य, बहुभाषी प्लेटफॉर्म में बदल देता है जिसमें एक [पब्लिक API](https://www.ufolens.com/api/v1) है। यह दो हिस्सों — एक लोकल Python इनजेस्ट पाइपलाइन (`pipeline/`) और एक TypeScript/Hono एज ऐप (`worker/`) — से बना है जो एक इंटरफेस पर मिलते हैं: एक पब्लिश्ड SQL + एसेट्स बंडल।

योगदान देने के लिए आपको किसी क्लाउड क्रेडेंशियल्स की आवश्यकता नहीं है। पाइपलाइन के कोर मॉड्यूल केवल stdlib-आधारित हैं और वर्कर टेस्ट इन-मेमोरी स्टोरेज के खिलाफ चलते हैं।

### सेटअप

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### कहाँ मदद सबसे अधिक उपयोगी है

**i18n / लोकलाइजेशन** — `worker/src/i18n/ui-strings.json` UI स्ट्रिंग्स का स्रोत है। किसी भी गैर-अंग्रेजी लोकैल (locale) की नेटिव-स्पीकर समीक्षा बहुत मूल्यवान है: अजीब मशीन आउटपुट को पकड़ना, RTL/लेआउट issues को ठीक करना, और लैंग्वेज-नेगोशिएशन एज केसेस में सुधार करना।

**OCR क्वालिटी** — OCR से पहले पुराने टाइपराइटर स्कैन की बेहतर प्री-प्रोसेसिंग; सैंपल पेजों पर ओपन-सोर्स इंजन बनाम Tesseract फॉलबैक की तुलना करने वाला इवैल्यूएशन हार्नेस।

**एक्सेसिबिलिटी (Accessibility)** — रेंडर किए गए पेजों (`worker/src/render/`) का WCAG के खिलाफ ऑडिट करें; CSP सख्त है (कोई `unsafe-inline` नहीं), इसलिए समाधान उसी के दायरे में काम करने चाहिए।

**API एर्गोनॉमिक्स** — `worker/src/routes/` — पेजिनेशन, फ़िल्टरिंग, OpenAPI विवरण, उदाहरण क्लाइंट्स।

**पाइपलाइन रोबस्टनेस (Pipeline robustness)** — अधिक ग्रेसफुल-डिग्रेडेशन पाथ्स, बेहतर प्रोग्रेस रिपोर्टिंग, डेल्टा-डिटेक्शन एज केसेस (`pipeline/lib/delta.py`)।

**डॉक्यूमेंट्स (Docs)** — `docs/20260511/` (繁體中文; `00-*` इंडेक्स है)। डिज़ाइन डॉक्स के अंग्रेजी अनुवाद का स्वागत है।

### बुनियादी नियम (Ground rules)

- प्रोजेक्ट के सापेक्ष — सभी पाथ मशीनों के बीच पोर्टेबल होने चाहिए। कोई हार्डकोडेड एब्सोल्यूट पाथ न रखें।
- पाइपलाइन के *कोर* मॉड्यूल में pip डिपेंडेंसी न जोड़ें। वैकल्पिक स्टेज वैकल्पिक पैकेज का उपयोग कर सकते हैं, और उनके बिना ग्रेसफुली डिग्रेड होने चाहिए।
- फॉरवर्ड-ओनली स्टेट मशीन — को कमजोर न करें, क्योंकि वह लागत की सीमा (cost ceiling) है।
- आधिकारिक अमेरिकी सरकार के प्रतीक चिन्ह (insignia) न जोड़ें, और ऐसा कुछ भी न जोड़ें जो सोर्स रिडक्शन (source redactions) को उलट दे।
- D1 स्कीमा परिवर्तन **दो** फाइलों को प्रभावित करते हैं: `pipeline/lib/manifest_schema.sql` और `db/schema.sql`।
- नए कोड के साथ टेस्ट शामिल करें। कन्वेंशनल-कमिट (Conventional-commit) मैसेज का उपयोग करें।

पहले `CLAUDE.md` और `docs/20260511/00-*` पढ़ें, फिर PR से पहले किसी भी संरचनात्मक (structural) चर्चा के लिए एक issue खोलें।