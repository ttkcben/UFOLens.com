# GitHub — 3 Gönderiden 3'ü · Mimari notları (ADR tarzı Tartışma)

**Kullanım amacı:** "Göster ve anlat" / "Mimari" altında bir Tartışma veya `docs/` ADR tohumu.
**Anahtar Kelimeler:** mimari, ADR, yalnızca ileriye dönük durum makinesi, yerel LLM, Ollama, OCR, edge bilişim, CSP, güvenlik başlıkları, veri işlem hattı, maliyet mühendisliği, SQLite manifestosu, D1, R2, KV
**Hiperlinkler:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com neden bu şekilde inşa edildi

[PURSUE UAP arşivinin](https://www.war.gov/ufo) aranabilir, çok dilli yeniden inşası olan [ufolens.com](https://www.ufolens.com)'u şekillendiren üç karar üzerine notlar. Yorumlar / karşı görüşler memnuniyetle karşılanır.

### 1. İşlem hattı kasıtlı olarak yalnızca ileriye dönük bir durum makinesidir

Durumlar: `discovered → downloaded → ocr_done → translated → published`. Bir belge yalnızca ileriye doğru hareket eder ve yalnızca yapılacak bir iş olduğunda. Yayınlanmış içerik, bir delta dedektörü kaynağın gerçekten değiştiğini görmedikçe asla yeniden işlenmez.

**Neden:** OCR + çeviri pahalı operasyonlardır ve arşiv zamanla büyür. "Güvende olmak için her şeyi yeniden çalıştıran" bir işlem hattının sınırsız maliyeti vardır. Geriye doğru geçişleri imkansız kılmak, kontrolden çıkmış bir faturayı imkansız kılar. Maliyet tavanı, operatörün uyanıklığının değil, durum grafiğinin bir özelliğidir.

**Maliyet:** şema geçişleri ve kasıtlı olarak yeniden işleme yapmak kasıtlı olarak zahmetlidir. Kabul edilebilir bir ödünleşim.

### 2. OCR ve çeviri bir bulut API'sinde değil, yerel bir LLM'de çalışır

OCR: açık kaynak motoru, Tesseract CLI yedeği. Çeviri + NER: Apple Silicon dizüstü bilgisayarda Ollama aracılığıyla Gemma.

**Neden:** belge başına sıfır marjinal maliyet; tekrarlanabilir (sabit model + istemler); ve getirme adımı zaten bir ev IP'sinden çalışmak zorunda (kaynak Akamai Bot Manager'ın arkasında — `curl` 403 alıyor), bu yüzden bir dizüstü bilgisayar zaten döngüde.

**Maliyet:** çeviri kalitesi, en son modelin altındadır. Orijinal İngilizce'nin her zaman bir tık uzakta olduğu bir referans külliyatı için bu sorun değil. Çevirilerin yetkili olduğunu iddia etmiyoruz.

### 3. İki yarı tam olarak tek bir arayüzü paylaşır: yayınlanmış bir paket

İşlem hattı hiçbir zaman doğrudan üretim veritabanına yazmaz. `{ SQL, varlık manifestosu, önbellek temizleme listesi }` yayar. "Yayınlama" = bu paketi ileriye doğru uygulamak (SQL'i edge SQL DB'ye itmek, varlıkları nesne depolama ile senkronize etmek, adlandırılmış önbellek anahtarlarını temizlemek).

**Neden:** yerel taraf ve edge tarafı bağımsız olarak gelişebilir; paket incelenebilir; ve "verileri dağıt" her seferinde aynı şekildedir. Worker küçük bir TypeScript/Hono uygulamasıdır — katı CSP (`unsafe-inline` yok; satır içi JSON-LD sha256 ile sabitlenmiştir), `Accept-Language` + ülke→dil anlaşması, 30 günlük KV sayfa önbelleği, günlük bakım cron'u — ve verilerin nasıl yapıldığını bilmesi gerekmez.

**Maliyet:** bir D1 şema değişikliği iki dosyayı etkiler (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Ucuz bir sigorta.

### Davranışa yerleşik olarak bulunan pazarlık edilemezler

- ABD hükümeti ile bağlantılı değildir; resmi amblem yoktur.
- Kaynak redaksiyonları korunur, asla geri alınmaz.
- Video DVIDS / AARO'ya atfedilmiştir.
- Site genelinde `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` — arama tarafından dizine eklenebilir, AI kazımasından vazgeçilmiştir.

Canlı: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

