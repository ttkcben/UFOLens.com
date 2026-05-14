# GitHub — Yazı 3-dən 2 · Töhfəkar çağırışı / "yaxşı ilk məsələlər"

**İstifadə forması:** bərkidilmiş Müzakirə ("Töhfə vermək və yaxşı ilk məsələlər") və ya CONTRIBUTING.md giriş.
**Açar sözlər:** açıq mənbə, töhfə vermək, yaxşı ilk məsələ, i18n, lokalizasiya, OCR, Python, TypeScript, Vitest, pytest, əlçatanlıq, UAP, açıq məlumat
**Hiperlinklər:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com-a töhfə vermək

[ufolens.com](https://www.ufolens.com) ABŞ Hərb Departamentinin [PURSUE UAP arxivini](https://www.war.gov/ufo) axtarıla bilən, çoxdilli bir platformaya və [ictimai API](https://www.ufolens.com/api/v1)-a çevirir. Bu, iki yarımdan ibarətdir — lokal Python qəbul boru kəməri (`pipeline/`) və TypeScript/Hono edge tətbiqi (`worker/`) — bir interfeysdə görüşür: dərc edilmiş SQL + aktivlər paketi.

Töhfə vermək üçün heç bir bulud etimadnaməsinə ehtiyacınız yoxdur. Boru kəmərinin əsas modulları yalnız stdlib-dən ibarətdir və Worker testləri yaddaşdaxili saxlanca qarşı işləyir.

### Quraşdırma

```bash
# pipeline
python3 -m pytest pipeline/tests/          # hamısı yaşıl olmalıdır, heç bir pip quraşdırılması lazım deyil

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Köməyin ən faydalı olduğu yerlər

**i18n / lokalizasiya** — `worker/src/i18n/ui-strings.json` UI sətirlərinin mənbəyidir. İngilis dili xaricindəki hər hansı bir lokalın ana dilində danışan tərəfindən nəzərdən keçirilməsi yüksək dəyərlidir: yöndəmsiz maşın çıxışını tutun, RTL/düzülüş problemlərini həll edin, dil danışıqlarının kənar hallarını yaxşılaşdırın.

**OCR keyfiyyəti** — OCR-dən əvvəl köhnə çap maşını ilə yazılmış skanların daha yaxşı ön-işlənməsi; nümunə səhifələrdə açıq mənbəli mühərriki Tesseract ehtiyatı ilə müqayisə edən qiymətləndirmə qurğusu.

**Əlçatanlıq** — göstərilən səhifələri (`worker/src/render/`) WCAG-a qarşı yoxlayın; CSP sərtdir (heç bir `unsafe-inline` yoxdur), buna görə həllər bu çərçivədə işləməlidir.

**API erqonomikası** — `worker/src/routes/` — səhifələmə, filtrləmə, OpenAPI təsviri, nümunə müştərilər.

**Boru kəməri möhkəmliyi** — daha çox səliqəli-pisləşmə yolları, daha yaxşı tərəqqi hesabatı, delta-aşkarlama kənar halları (`pipeline/lib/delta.py`).

**Sənədlər** — `docs/20260511/` (繁體中文; `00-*` indeksdir). Dizayn sənədlərinin İngilis dilinə tərcümələri xoş qarşılanır.

### Əsas qaydalar

- Bütün yollar nisbidir — layihə maşınlar arasında daşına bilən olmalıdır. Heç bir sərt kodlanmış mütləq yol yoxdur.
- Bir boru kəməri *nüvə* moduluna pip asılılığı əlavə etməyin. Könüllü mərhələlər könüllü paketlərdən istifadə edə bilər və onlarsız səliqəli şəkildə pisləşməlidir.
- Yalnız irəliyə doğru işləyən vəziyyət maşınını zəiflətməyin — bu, xərc tavanıdır.
- Rəsmi ABŞ hökumət nişanlarını əlavə etməyin və mənbə redaktələrini geri qaytaran heç bir şey əlavə etməyin.
- D1 sxem dəyişiklikləri **iki** fayla toxunur: `pipeline/lib/manifest_schema.sql` və `db/schema.sql`.
- Yeni kodla testlər. Adi-öhdəlik mesajları.

Əvvəlcə `CLAUDE.md` və `docs/20260511/00-*` sənədlərini oxuyun, sonra PR-dan əvvəl hər hansı bir struktur dəyişikliyi müzakirə etmək üçün bir məsələ açın.
