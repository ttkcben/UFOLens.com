# GitHub — Yazı 3-dən 3 · Arxitektura qeydləri (ADR-üslubunda Müzakirə)

**İstifadə forması:** "Göstər və danış" / "Arxitektura" altında bir Müzakirə və ya `docs/` ADR toxumu.
**Açar sözlər:** arxitektura, ADR, yalnız irəliyə doğru vəziyyət maşını, lokal LLM, Ollama, OCR, edge computing, CSP, təhlükəsizlik başlıqları, məlumat boru kəməri, xərc mühəndisliyi, SQLite manifesti, D1, R2, KV
**Hiperlinklər:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Niyə ufolens.com bu şəkildə qurulub

[ufolens.com](https://www.ufolens.com)-u ([PURSUE UAP arxivinin](https://www.war.gov/ufo) axtarıla bilən, çoxdilli yenidən qurulması) formalaşdıran üç qərar haqqında qeydlər. Şərhlər / etirazlar xoş qarşılanır.

### 1. Boru kəməri qəsdən yalnız irəliyə doğru işləyən bir vəziyyət maşınıdır

Vəziyyətlər: `kəşf edildi → endirildi → ocr_tamamlandı → tərcümə edildi → dərc edildi`. Bir sənəd yalnız irəli hərəkət edir və yalnız görüləcək iş olduqda. Dərc edilmiş məzmun, bir delta detektoru mənbəyin həqiqətən dəyişdiyini görməyincə heç vaxt yenidən işlənmir.

**Niyə:** OCR + tərcümə bahalı əməliyyatlardır və arxiv zamanla böyüyür. "Təhlükəsiz olmaq üçün hər şeyi yenidən işlədən" bir boru kəmərinin məhdudiyyətsiz xərci var. Geri keçidləri qeyri-mümkün etmək, idarəolunmaz bir hesabı qeyri-mümkün edir. Xərc tavanı operatorun sayıqlığının deyil, vəziyyət qrafikinin bir xüsusiyyətidir.

**Maliyəti:** sxem miqrasiyaları və qəsdən yenidən işləmə qəsdən yöndəmsizdir. Qəbul edilən bir güzəşt.

### 2. OCR və tərcümə bulud API-sində deyil, lokal bir LLM-də işləyir

OCR: açıq mənbəli mühərrik, Tesseract CLI ehtiyatı. Tərcümə + NER: Gemma vasitəsilə Ollama, bir Apple Silicon noutbukunda.

**Niyə:** sənəd başına sıfır marjinal xərc; təkrarlana bilən (sabit model + təlimatlar); və `fetch` addımı onsuz da bir yaşayış IP-sindən işləməlidir (mənbə Akamai Bot Manager-in arxasındadır — `curl` 403 alır), buna görə bir noutbuk onsuz da dövrədədir.

**Maliyəti:** tərcümə keyfiyyəti bir sərhəd modelindən aşağıdır. Orijinal İngilis dilinin həmişə bir klik uzaqda olduğu bir istinad korpusu üçün bu yaxşıdır. Tərcümələrin səlahiyyətli olduğunu iddia etmirik.

### 3. İki yarım tam olaraq bir interfeys paylaşır: dərc edilmiş bir paket

Boru kəməri heç vaxt birbaşa istehsalat verilənlər bazasına yazmır. O, `{ SQL, aktiv manifesti, keş təmizləmə siyahısı }` çıxarır. "Dərc etmək" = bu paketi irəli tətbiq etmək (SQL-i edge SQL DB-ə təkan vermək, aktivləri obyekt saxlancına sinxronlaşdırmaq, adlandırılmış keş açarlarını təmizləmək).

**Niyə:** lokal tərəf və edge tərəfi müstəqil şəkildə inkişaf edə bilər; paket nəzərdən keçirilə bilər; və "məlumatları yerləşdirmək" hər dəfə eyni formadadır. Worker kiçik bir TypeScript/Hono tətbiqidir — ciddi CSP (heç bir `unsafe-inline`; daxili JSON-LD sha256-ilə bərkidilib), `Accept-Language` + ölkə→dil danışıqları, 30 günlük KV səhifə keşi, gündəlik təmizlik cronu — və məlumatların necə yaradıldığını heç vaxt bilməsinə ehtiyac yoxdur.

**Maliyəti:** bir D1 sxem dəyişikliyi iki fayla toxunur (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Ucuz sığorta.

### Davranışa daxil edilmiş müzakirəolunmaz məqamlar

- ABŞ hökuməti ilə əlaqəli deyil; heç bir rəsmi nişan yoxdur.
- Mənbə redaktələri qorunur, heç vaxt geri qaytarılmır.
- Video DVIDS / AARO-ya aiddir.
- Bütün saytda `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` — axtarışa indekslənə bilən, AI qaşınmasından imtina edilmiş.

Canlı: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
