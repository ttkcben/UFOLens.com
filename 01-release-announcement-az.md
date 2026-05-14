# GitHub — Yazı 3-dən 1 · Buraxılış / README elan bloku

**İstifadə forması:** GitHub Buraxılış mətni, bərkidilmiş Müzakirə və ya repo README-nin yuxarı hissəsi.
**Açar sözlər:** UAP, UFO, PURSUE arxivi, məxfiliyi ləğv edilmiş sənədlər, açıq məlumat, tam mətnli axtarış, OCR, maşın tərcüməsi, lokal LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Hiperlinklər:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP arxivi üçün çoxdilli, axtarışa imkan verən platforma

**Canlı:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Mənbə arxivi:** https://www.war.gov/ufo

`ufolens.com` ABŞ Hərb Departamentinin məxfiliyi ləğv edilmiş UAP / UFO qeydlərindən ibarət **PURSUE** arxivini bir bilik platforması kimi yenidən dərc edir: tam mətnli axtarış, bütün korpus üzrə maşın tərcüməsi, xəritə + zaman xətti ilə kəşfiyyat və ictimai JSON API. Mənbə sənədləri ABŞ federal hökumətinin işləridir və ABŞ daxilində ictimai mülkiyyətdədir ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Bu layihənin **ABŞ hökuməti ilə heç bir əlaqəsi yoxdur**, heç bir rəsmi nişandan istifadə etmir və redaktə edilmiş yerləri heç vaxt bərpa etmir.

### Arxitektura

```
Lokal maşın (Apple Silicon, yaşayış IP)        Edge şəbəkəsi
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only nüvə)         worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (yalnız irəli)   /{lang}/...   səhifələr
  OCR: açıq mənbəli mühərrik (Tesseract CLI ehtiyat)  /api/v1/...   ictimai API
  translate / NER: lokal LLM (Gemma via Ollama)      /admin        operator konsolu
  vəziyyət: SQLite manifesti                        dəstəklənir: edge SQL DB, obyekt
        │                                              saxlanc (mənbə PDF-lər), KV keş
        └── bir paket dərc edir: SQL + aktiv manifesti + keş təmizləmə siyahısı ──┘
```

- **Sənəd başına sıfır bulud-AI xərci.** OCR və tərcümə lokal olaraq işləyir; yalnız irəliyə doğru işləyən vəziyyət maşını (`kəşf edildi → endirildi → ocr_tamamlandı → tərcümə edildi → dərc edildi`) sənədin dəyişmədiyi təqdirdə yenidən işlənməməsini təmin edir.
- **Boru kəməri nüvəsinin heç bir üçüncü tərəf asılılığı yoxdur** — təhlil / manifest / delta modulları heç bir `pip` quraşdırılması olmadan təmiz Python-da işləyir və test edilir; OCR/tərcümə mərhələləri əlavə paketlər olmadıqda səliqəli şəkildə pisləşir.
- **Edge saytı** ciddi təhlükəsizlik başlıqları + CSP (heç bir `unsafe-inline`; daxili JSON-LD sha256 ilə bərkidilib), `Accept-Language` + ölkə xəritəçəkməsi vasitəsilə dil danışıqları, 30 günlük KV səhifə keşi və gündəlik təmizlik cronu tətbiq edir.
- **Artımlı yeniləmələr:** bir delta detektoru mənbə indeksini fərqləndirir və yalnız dəyişiklikləri boru kəmərinə geri ötürür.

### Developerlər üçün

https://www.ufolens.com/api/v1 ünvanındakı ictimai API sənədləri və metadatanı JSON formatında qaytarır. Anonim giriş dərəcəsi məhduddur; tədqiqatçı/developer səviyyələri üçün bir açar tələb edin. Endpointlər və limitlər üçün saytdakı API bölməsinə baxın.

### Status

Kod tamamlanıb; sayt https://www.ufolens.com ünvanında yerləşdirilib. İstehsalat verilənlər bazası oflayn boru kəmərini işə salmaqla və paketi irəli dərc etməklə (`cli_publish run --remote`) doldurulur. Tam dizayn sənədləri `docs/20260511/` qovluğunda yaşayır.

### Lisenziya / sərhədlər

- Mənbə sənədləri: ABŞ federal hökumətinin işləri, ABŞ daxilində ictimai mülkiyyət.
- Bu platformanın öz kodu: `LICENSE` faylına baxın.
- Sayt `Tdm-Reservation: 1` və `X-Robots-Tag: noai, noimageai` göndərir — axtarış motorları tərəfindən indekslənə bilər, AI təlimi/qaşınmasından imtina edilib.
- Video görüntülər DVIDS / AARO-ya aiddir və bu layihə tərəfindən iddia edilmir.

Məsələlər və PR-lar xoş qarşılanır. Struktur dəyişiklikləri açmazdan əvvəl `CLAUDE.md` və `docs/20260511/00-*` sənədlərini oxuyun.
