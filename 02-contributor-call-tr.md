# GitHub — 3 Gönderiden 2'si · Katılımcı çağrısı / "iyi ilk konular"

**Kullanım amacı:** Sabitlenmiş bir Tartışma ("Katkıda Bulunma ve iyi ilk konular") veya bir CONTRIBUTING.md girişi.
**Anahtar Kelimeler:** açık kaynak, katkıda bulunma, iyi ilk konu, i18n, yerelleştirme, OCR, Python, TypeScript, Vitest, pytest, erişilebilirlik, UAP, açık veri
**Hiperlinkler:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com'a Katkıda Bulunma

[ufolens.com](https://www.ufolens.com), ABD Savaş Bakanlığı'nın [PURSUE UAP arşivini](https://www.war.gov/ufo) aranabilir, çok dilli bir platforma ve [halka açık bir API'ye](https://www.ufolens.com/api/v1) dönüştürür. İki yarıdan oluşur — yerel bir Python alım işlem hattı (`pipeline/`) ve bir TypeScript/Hono edge uygulaması (`worker/`) — tek bir arayüzde buluşur: yayınlanmış bir SQL + varlık paketi.

Katkıda bulunmak için herhangi bir bulut kimlik bilgisine ihtiyacınız yoktur. İşlem hattının çekirdek modülleri yalnızca stdlib'dir ve Worker testleri bellek içi depolamaya karşı çalışır.

### Kurulum

```bash
# pipeline
python3 -m pytest pipeline/tests/          # hepsi yeşil olmalı, pip kurulumu gerekmez

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Yardım en çok nerede faydalı olur?

**i18n / yerelleştirme** — `worker/src/i18n/ui-strings.json`, kullanıcı arayüzü dizelerinin kaynağıdır. İngilizce olmayan herhangi bir yerel ayarın ana dili konuşmacısı tarafından gözden geçirilmesi yüksek değerlidir: garip makine çıktılarını yakalayın, RTL/düzen sorunlarını düzeltin, dil anlaşmasıyla ilgili uç durumları iyileştirin.

**OCR kalitesi** — OCR öncesi eski daktilo taramalarının daha iyi ön işlenmesi; açık kaynak motorunu örnek sayfalardaki Tesseract yedeğiyle karşılaştıran bir değerlendirme mekanizması.

**Erişilebilirlik** — oluşturulan sayfaları (`worker/src/render/`) WCAG'ye göre denetleyin; CSP katıdır (`unsafe-inline` yok), bu nedenle çözümler bu çerçevede çalışmalıdır.

**API ergonomisi** — `worker/src/routes/` — sayfalama, filtreleme, OpenAPI açıklaması, örnek istemciler.

**İşlem hattı sağlamlığı** — daha zarif devre dışı kalma yolları, daha iyi ilerleme raporlaması, delta algılama uç durumları (`pipeline/lib/delta.py`).

**Belgeler** — `docs/20260511/` (繁體中文; `00-*` dizindir). Tasarım belgelerinin İngilizce'ye çevirileri memnuniyetle karşılanır.

### Temel kurallar

- Tüm yollar görecelidir — proje makineler arasında taşınabilir olmalıdır. Sabit kodlanmış mutlak yol yoktur.
- Bir işlem hattı *çekirdek* modülüne bir pip bağımlılığı eklemeyin. İsteğe bağlı aşamalar isteğe bağlı paketleri kullanabilir ve bunlar olmadan zarif bir şekilde devre dışı kalmalıdır.
- Yalnızca ileriye dönük durum makinesini zayıflatmayın — bu, maliyet tavanıdır.
- Resmi ABD hükümeti amblemlerini tanıtmayın ve kaynak redaksiyonlarını geri alan hiçbir şey eklemeyin.
- D1 şema değişiklikleri **iki** dosyayı etkiler: `pipeline/lib/manifest_schema.sql` ve `db/schema.sql`.
- Yeni kodla birlikte testler. Conventional-commit mesajları.

Önce `CLAUDE.md` ve `docs/20260511/00-*` dosyalarını okuyun, ardından PR'dan önce herhangi bir yapısal şeyi tartışmak için bir konu açın.

