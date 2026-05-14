# GitHub — პოსტი 1/3 · რელიზი / README-ს განცხადების ბლოკი

**გამოიყენეთ, როგორც:** a GitHub Release-ის ტექსტი, ჩამაგრებული დისკუსია, ან რეპოზიტორიის README-ს თავში.
**საკვანძო სიტყვები:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**ჰიპერბმულები:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP არქივის მრავალენოვანი, საძიებო პლატფორმა

**ლაივი:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **საწყისი არქივი:** https://www.war.gov/ufo

`ufolens.com` ხელახლა აქვეყნებს აშშ-ის ომის დეპარტამენტის **PURSUE** არქივს, რომელიც შეიცავს გასაიდუმლოებულ UAP / UFO ჩანაწერებს, როგორც ცოდნის პლატფორმას: სრულტექსტოვანი ძიება, კორპუსის მასშტაბით მანქანური თარგმანი, რუკისა და ქრონოლოგიის კვლევა, და საჯარო JSON API. საწყისი დოკუმენტები არის აშშ-ის ფედერალური მთავრობის ნამუშევრები და აშშ-ის ტერიტორიაზე საჯარო დომენშია ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). ეს პროექტი **არ არის დაკავშირებული აშშ-ის მთავრობასთან**, არ იყენებს ოფიციალურ ნიშნებს და არასდროს არ ხსნის რედაქტირებულ ნაწილებს.

### არქიტექტურა

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

- **ნულოვანი ღირებულება დოკუმენტზე ღრუბლოვანი AI-სთვის.** OCR და თარგმანი მუშაობს ლოკალურად; მხოლოდ წინსვლის მდგომარეობათა მანქანა (`discovered → downloaded → ocr_done → translated → published`) იძლევა გარანტიას, რომ დოკუმენტი ხელახლა არ დამუშავდება, თუ ის არ შეცვლილა.
- **Pipeline-ის ბირთვს არ აქვს მესამე მხარის დამოკიდებულებები** — პარსინგის / მანიფესტის / დელტას მოდულები მუშაობს და ტესტირდება სუფთა Python-ზე, ყოველგვარი pip-ინსტალაციის გარეშე; OCR/თარგმანის ეტაპები შეუფერხებლად მუშაობს, როდესაც ოპციონალური პაკეტები არ არის.
- **Edge საიტი** იყენებს მკაცრ უსაფრთხოების ჰედერებს + CSP (არ არის `unsafe-inline`; inline JSON-LD არის sha256-ით დამაგრებული), ენის შეთანხმებას `Accept-Language` + ქვეყნის რუკების მეშვეობით, 30-დღიან KV გვერდის ქეშს და ყოველდღიურ cron-ს დასუფთავებისთვის.
- **ინკრემენტული განახლებები:** დელტა დეტექტორი ადარებს საწყის ინდექსს და მხოლოდ ცვლილებებს აწვდის pipeline-ს.

### დეველოპერებისთვის

საჯარო API https://www.ufolens.com/api/v1-ზე აბრუნებს დოკუმენტებს და მეტამონაცემებს JSON ფორმატში. ანონიმურ წვდომას აქვს რეიტ-ლიმიტი; მოითხოვეთ გასაღები მკვლევარის/დეველოპერის დონეებისთვის. იხილეთ API-ს სექცია საიტზე ენდფოინთებისა და ლიმიტების შესახებ.

### სტატუსი

კოდი დასრულებულია; საიტი განთავსებულია https://www.ufolens.com-ზე. წარმოების მონაცემთა ბაზა ივსება ოფლაინ pipeline-ის გაშვებით და პაკეტის წინსვლით გამოქვეყნებით (`cli_publish run --remote`). სრული დიზაინის დოკუმენტები განთავსებულია `docs/20260511/`-ში.

### ლიცენზია / საზღვრები

- საწყისი დოკუმენტები: აშშ-ის ფედერალური მთავრობის ნამუშევრები, საჯარო დომენი აშშ-ის ფარგლებში.
- ამ პლატფორმის საკუთარი კოდი: იხილეთ `LICENSE`.
- საიტი აგზავნის `Tdm-Reservation: 1`-ს და `X-Robots-Tag: noai, noimageai`-ს — ინდექსირებადია საძიებო სისტემებისთვის, მაგრამ უარს ამბობს AI ტრენინგზე/სკრეიპინგზე.
- ვიდეო მასალა მიეკუთვნება DVIDS / AARO-ს და ამ პროექტის მიერ არ არის მოთხოვნილი.

Issues და PR-ები მისასალმებელია. გთხოვთ, წაიკითხოთ `CLAUDE.md` და `docs/20260511/00-*` სტრუქტურული ცვლილებების გახსნამდე.
