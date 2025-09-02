
➡ **BASE**, NoSQL veritabanlarının tutarlılık yaklaşımını tanımlayan bir ilkeler bütünüdür.  
➡ **ACID** (SQL’in dayandığı prensipler) katı tutarlılık sağlarken, **BASE** esnek ve ölçeklenebilir bir tutarlılık sunar.  
➡ BASE’in açılımı şudur:

|Harf|Açılım|Anlamı|
|---|---|---|
|**B**|Basically Available|Sistem, her zaman isteklerin çoğuna yanıt verir (tam yanıt değilse bile boş veya hata dönebilir).|
|**A**|Soft-state|Veritabanının durumu zamanla değişebilir; her an tutarlılık garantisi yoktur.|
|**SE**|Eventually consistent|Veriler zamanla (eventually) tutarlı hale gelir; anlık tutarlılık beklenmez.|

---

### 🔹 **BASE Detaylı Açıklama**

|Özellik|Açıklama|
|---|---|
|**Basically Available (Temel erişilebilirlik)**|Sistem çalışmaya devam eder; tüm düğümlerde veri tutarlı olmasa bile çoğu istek yanıtlanır.|
|**Soft-state (Yumuşak durum)**|Sistemin durumu her zaman sabit değildir. Veriler, kopyalar arasında tutarsız olabilir ve bu normal kabul edilir.|
|**Eventually consistent (Sonunda tutarlılık)**|Veriler ilk etapta farklı düğümlerde farklı olabilir; ama sistem kendi içinde senkronize olur ve sonunda tutarlılığı sağlar.|

---

### 🔹 **BASE’in Amacı**

➡ Büyük, dağıtık sistemlerde **hız**, **ölçeklenebilirlik** ve **yüksek erişilebilirlik** sağlamak.  
➡ Kullanıcılar için “her zaman hızlı bir yanıt almak” **anında tutarlılıktan** daha önemlidir.

---

## 📌 **BASE vs ACID Karşılaştırması**

Şimdi ACID ile BASE’i yanyana görelim:

|Özellik|ACID (SQL)|BASE (NoSQL)|
|---|---|---|
|**Atomicity**|Tüm işlem ya tamamlanır ya hiç olmaz|Tek seferde tam atomiklik yok; parçalar halinde işler|
|**Consistency**|Her işlem veriyi geçerli bir duruma taşır|Zaman içinde tutarlılık (eventual consistency)|
|**Isolation**|İşlemler birbirinden bağımsız yürür|Daha az izolasyon; eşzamanlılıkta esneklik|
|**Durability**|İşlem kalıcıdır, sistem çökse bile kaybolmaz|Kalıcılık garantisi daha gevşek (bazı NoSQL sistemlerde opsiyonel)|
|**Erişilebilirlik**|Tutarlılık uğruna erişilebilirlikten taviz verir|Erişilebilirlik ön planda, tutarlılık sonra gelir|
|**Kullanım Alanı**|Bankacılık, finans, kritik sistemler|Sosyal medya, IoT, büyük veri, içerik yönetimi|

---

## 📝 **Özet**

- **ACID**: “Doğru olsun, yavaş olsa da olur.”
- **BASE**: “Hızlı olsun, zamanla doğru hale gelsin.”

---

