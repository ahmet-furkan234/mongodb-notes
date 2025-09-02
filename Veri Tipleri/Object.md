
MongoDB’de **Object** (veya Embedded Document), bir alanın değerinin kendi içinde anahtar-değer çiftlerinden oluşan başka bir belge (sub-document) olmasıdır.

➡ Yani bir belgenin içinde başka bir belge barındırılır.  
➡ İç içe yapı (nested structure) sağlar.

---

## 🌟 **Özellikler**

✅ Embedded object içindeki alanlar, üst belgeyle birlikte saklanır.  
✅ Alt alanlara doğrudan erişim ve sorgu mümkündür (ör. `adres.sehir`).  
✅ İç içe belgeler her belge için farklı yapılarda olabilir (şema zorunluluğu yok).

---

## 📝 **Örnek Belge**

```json
{
  "ad": "Ahmet",
  "adres": {
    "sehir": "Ankara",
    "postaKodu": "06000",
    "ulke": "Türkiye"
  }
}
```

➡ `adres` alanı bir embedded object’tir.

---

## ⚡ **Embedded Object Üzerinde Yapılabilecek İşlemler**

### 1️⃣ **Alt alana göre arama**

```js
db.kisiler.find({ "adres.sehir": "Ankara" })
```

➡ `adres` içindeki `sehir` alanı "Ankara" olan belgeleri getirir.

---

### 2️⃣ **Alt alan üzerine index**

```js
db.kisiler.createIndex({ "adres.sehir": 1 })
```

➡ adres.sehir alanında sorguları hızlandırır.

---

### 3️⃣ **Alt belgeyi güncelleme**

```js
db.kisiler.updateOne(
  { "ad": "Ahmet" },
  { $set: { "adres.postaKodu": "06100" } }
)
```

➡ `adres.postaKodu` alanını günceller.

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Embedded Object|SQL Karşılığı|
|---|---|---|
|Yapı|Aynı belgede alt belge|Ayrı tablo + foreign key|
|Okuma|✔ Tek belge erişimi|❌ JOIN gerekir|
|Yazma|✔ Tek belge değişir|❌ Ana tablo + alt tablo değişir|
|Bütünlük|❌ Geliştirici kontrolünde|✔ Foreign key ile sağlanır|
|Sorgu|✔ Alt alan doğrudan sorgulanabilir|❌ JOIN + WHERE ile sorgulanır|
|Şema|✔ Esnek|❌ Sabit şema|

---

### 📝 **SQL’de bu yapı şöyle olurdu**

```sql
-- Ana tablo
id | ad
---|-----
1  | Ahmet

-- Adres tablosu
id | kisi_id | sehir  | postaKodu | ulke
---|---------|--------|-----------|------
1  | 1       | Ankara | 06000     | Türkiye
```

➡ SQL’de ayrı tablo + JOIN gerekir.

---

## 🌟 **Avantaj (MongoDB Embedded Object)**

✅ JOIN ihtiyacını ortadan kaldırır → daha hızlı okuma  
✅ Tek belge değişir → daha az yazma işlemi  
✅ Alt alanlara doğrudan erişim ve indexleme mümkün  
✅ Şema esnek → her belgede farklı alanlar olabilir

---

## 🚩 **Dezavantaj (MongoDB Embedded Object)**

❌ Büyük belgeler büyüdükçe 16MB sınırına yaklaşabilirsin  
❌ Alt belgede sık tekrar eden veri değişirse hepsini güncellemek zorunda kalırsın  
❌ İlişkiler karmaşıklaşırsa yönetimi zorlaşır  
❌ Veri bütünlüğü (ör. referans kontrolü) geliştiriciye kalır

---

## 🎯 **Ne zaman Embedded Object tercih edilir?**

👉 Alt belge üst belgeyle sıkı bağlıysa (ör: adres, sipariş detayları)  
👉 Alt veriyi bağımsız güncellemen gerekmiyorsa  
👉 Sorgularda sık sık alt veriye ihtiyacın varsa