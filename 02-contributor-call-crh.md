# GitHub — 2-nci yazı (3-ten) · İsse qoşuv çağırısı / "yahşı birinci meseleler"

**Qullanım:** Sabitleştirilgen bir Muzakere ("İsse qoşuv & yahşı birinci meseleler") ya da bir CONTRIBUTING.md kirişi olaraq.
**Açar süzler:** açıq kod, isse qoşuv, yahşı birinci mesele, i18n, yerlileştirüv, OCR, Python, TypeScript, Vitest, pytest, irişimlik, UAP, açıq malümat
**Giperbağlantılar:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com'ğa isse qoşuv

[ufolens.com](https://www.ufolens.com), ABD Cenkleşüv Departamenti'niñ [PURSUE UAP arhivini](https://www.war.gov/ufo), [umumi bir API](https://www.ufolens.com/api/v1) ile qıdıruv imkânı olğan, çoq tilli bir platformağa çevire. O, eki yarımdan ibaret — yerli bir Python aluv boru hattı (`pipeline/`) ve bir TypeScript/Hono kenar uygulaması (`worker/`) — bir interfeyste birleşe: neşir etilgen bir SQL + varlıqlar paketi.

İsse qoşmaq içün iç bir bulut kimlik malümatına ihtiyacıñız yoqtır. Boru hattınıñ öz modulleri tek stdlib'ğa bağlıdır ve Worker testleri hafızadaki saqlavğa qarşı çalışa.

### Quruştıruv

```bash
# boru hattı
python3 -m pytest pipeline/tests/          # episi yeşil olmalı, pip qurulumı kerekmiy

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Yardım eñ faydalı olğan yerler

**i18n / yerlileştirüv** — `worker/src/i18n/ui-strings.json` UI satırlarınıñ menbasıdır. İngilizceden başqa er angi bir tilniñ ana tilinde laf etken biri tarafından közden keçirilmesi pek qıymetlidir: beceriksiz maşna çıqtılarını tapıñ, RTL/maket mesellerini tüzetiñ, til muzakeresindeki kenar vaziyetlerini yahşılatıñ.

**OCR keyfiyeti** — OCR'dan evel eski daktilo ile yazılğan skanlarnıñ daa yahşı işlenmesi; örnek saifelerde açıq kodlu motor ile Tesseract yedegini qıyaslağan bir değerlendirme sistemi.

**İrişimlik** — işlengen saifelerni (`worker/src/render/`) WCAG'ğa qarşı teşkermeli; CSP qattıdır (`unsafe-inline` yoq), bu sebepten çareler bu çerçive içinde çalışmalı.

**API ergonomikası** — `worker/src/routes/` — saifelev, süzgüçlev, OpenAPI tasviri, örnek klientler.

**Boru hattı sağlamlığı** — daa yahşı zarif eksik çalışma yolları, daa yahşı ilerleme bildirüvi, delta-tespit etüv kenar vaziyetleri (`pipeline/lib/delta.py`).

**Vesiqalar** — `docs/20260511/` (繁體中文; `00-*` indeksidir). Dizayn vesiqalarınıñ İngilizce tercimeleri qabul etile.

### Esas qaideler

- Bütün yollar nisbiydir — leyha maşnalar arasında taşınabilir olmalı. İç bir qattı mutlaq yol kodlanmamalıdır.
- Bir boru hattı *öz* modülüne bir pip bağlılığı eklemeñiz. İstege bağlı basamaqlar istege bağlı paketler qullanabilir ve bularsız zarif bir şekilde eksik çalışmalıdır.
- Tek ileri areket etken vaziyet maşnasını zayıflatmañız — bu, masrafnıñ üst sıñırıdır.
- Resmiy ABD ükümet timsallerini kirsetmeñiz ve asıl redaktsiyalarnı keri çevirgen iç bir şey eklemeñiz.
- D1 şema deñişiklikleri **eki** faylğa tesir ete: `pipeline/lib/manifest_schema.sql` ve `db/schema.sql`.
- Yañı kod ile testler. Conventional-commit mesajları.

Bir PR'dan evel er angi bir strukturalı deñişiklikni muzakere etmek içün `CLAUDE.md` ve `docs/20260511/00-*` fayllarını oquñız, soñra bir mesele açıñız.

