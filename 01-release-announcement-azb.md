# GitHub — 3-dən 1-ci Yazı · Çıxarış / README elan bloku

**İstifadə edin:** GitHub Buraxılış gövdəsi, sancılmış Müzakirə və ya repo README-nin yuxarısı kimi.
**Açar sözlər:** UAP, UFO, PURSUE arxivi, məxfiliyi qaldırılmış sənədlər, açıq məlumat, tam mətn axtarışı, OCR, maşın tərcüməsi, yerli LLM, Ollama, kənar hesablama, ictimai API, Hono, TypeScript, Python
**Hiperlinklər:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP arxivı üçün çoxdilli, axtarış edilə bilən platforma

**Canlı:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Mənbə arxivi:** https://www.war.gov/ufo

`ufolens.com` ABŞ Müharibə Nazirliyinin məxfiliyi qaldırılmış UAP / UFO qeydləri olan **PURSUE** arxivini bilik platforması olaraq yenidən nəşr edir: tam mətn axtarışı, korpus boyunca maşın tərcüməsi, xəritə + zaman xətti tədqiqi və ictimai JSON API. Mənbə sənədləri ABŞ federal hökumətinin əsərləridir və ABŞ daxilində ictimai mülkiyyətdir ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Bu layihə **ABŞ hökuməti ilə əlaqəli deyil**, heç bir rəsmi nişandan istifadə etmir və heç vaxt redaktələri geri çevirmir.

### Memarlıq

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

- **Sənəd başına sıfır bulud-AI xərci.** OCR və tərcümə yerli olaraq işləyir; yalnız irəli istiqamətli vəziyyət maşını (`discovered → downloaded → ocr_done → translated → published`) sənədin dəyişməsi halında yenidən işlənməməsinə zəmanət verir.
- **Pipeline əsası üçüncü tərəf asılılıqlarına malik deyil** — parsing / manifest / delta modulları təmiz Python mühitində pip quraşdırılması olmadan işləyir və sınaqdan keçir; OCR/tərcümə mərhələləri əlavə paketlər olmadıqda zərif şəkildə pisləşir.
- **Kənar sayt** sərt təhlükəsizlik başlıqlarını + CSP-ni (no `unsafe-inline`; inline JSON-LD sha256 ilə sabitləndi), `Accept-Language` + ölkə xəritəsi vasitəsilə dil danışıqlarını, 30 günlük KV səhifə keşini və gündəlik təmizləmə cronunu tətbiq edir.
- **Artırımlı yeniləmələr:** delta detektoru mənbə indeksini fərqləndirir və yalnız dəyişiklikləri yenidən pipeline-a ötürür.

### İnkişaf etdiricilər üçün

https://www.ufolens.com/api/v1 ünvanında yerləşən ictimai API sənədləri və metadata-nı JSON olaraq qaytarır. Anonim giriş sürət məhdudiyyətlidir; tədqiqatçı/inkişaf etdirici səviyyələri üçün açar tələb edin. Son nöqtələr və məhdudiyyətlər üçün saytın API bölməsinə baxın.

### Status

Kod tamamlanıb; sayt https://www.ufolens.com ünvanında yerləşdirilib. İstehsal verilənlər bazası offline pipeline-ı işə salmaq və paketi irəli nəşr etməklə doldurulur (`cli_publish run --remote`). Tam dizayn sənədləri `docs/20260511/` ünvanında yerləşir.

### Lisenziya / sərhədlər

- Mənbə sənədləri: ABŞ federal hökumətinin əsərləri, ABŞ daxilində ictimai mülkiyyət.
- Bu platformanın öz kodu: `LICENSE`-ə baxın.
- Sayt `Tdm-Reservation: 1` və `X-Robots-Tag: noai, noimageai` göndərir — axtarış motorları tərəfindən indekslənilə bilər, AI təlimi/skrepinqindən imtina edildi.
- Video kadrları DVIDS / AARO-ya aid edilir və bu layihə tərəfindən iddia edilmir.

Məsələlər və PR-lər xoş gəlmisiniz. Struktur dəyişiklikləri açmadan əvvəl `CLAUDE.md` və `docs/20260511/00-*` sənədlərini oxuyun.

