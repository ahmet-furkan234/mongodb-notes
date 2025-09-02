
MongoDB’de **Double**, ondalıklı (kayan noktalı) sayıları saklamak için kullanılan bir sayısal veri tipidir.  
➡ **IEEE 754 64-bit floating point** standardına uygundur (SQL’deki `FLOAT` veya `DOUBLE PRECISION` gibidir).

---

## 🌟 **Özellikler**

✅ 64-bit genişliğe sahip → çok büyük ve çok küçük ondalıklı sayıları hassas bir şekilde saklayabilir.  
✅ Sayısal hesaplamalarda ondalıklı değere ihtiyaç duyduğun yerde kullanılır.  
✅ Sorgularda aritmetik ve karşılaştırma işlemleri yapılabilir (`$gt`, `$lt`, `$eq`).

---

## 📝 **Örnek Belge**

```json
{
  "urun": "Kalem",
  "fiyat": 2.75
}
```

➡ Burada `fiyat` alanı Double tipinde tutulur.

---

## ⚡ **Double Üzerinde Yapılabilecek İşlemler**

### 1️⃣ **Karşılaştırma**

```js
db.urunler.find({ "fiyat": { $gt: 10.5 } })
```

➡ Fiyatı 10.5’tan büyük ürünleri getirir.

---

### 2️⃣ **Sıralama**

```js
db.urunler.find().sort({ "fiyat": 1 })
```

➡ Fiyatına göre artan sıralama yapar.

---

### 3️⃣ **Aggregation ile toplama**

```js
db.urunler.aggregate([
  { $group: { _id: null, toplamFiyat: { $sum: "$fiyat" } } }
])
```

➡ Tüm ürünlerin fiyatlarının toplamını hesaplar.

---

## ⚠️ **Dikkat Edilecek Noktalar**

- 🔹 Double hassasiyetinde klasik _kayan nokta hataları_ oluşabilir (ör. 0.1 + 0.2 tam 0.3 olmayabilir).
- 🔹 **Decimal128** çok yüksek hassasiyet gereken finansal işlemlerde tercih edilir.
- 🔹 Double ile int karşılaştırmalarında tip farklarına dikkat et (ör. `10` int ile `10.0` double farklı tipte kabul edilir).

---

## 🎯 **SQL ile Kıyas**

|Özellik|MongoDB Double|SQL FLOAT / DOUBLE|
|---|---|---|
|Boyut|64 bit (IEEE 754)|SQL'de FLOAT / DOUBLE ile aynı|
|Hassasiyet|Ondalıklı → kayan nokta hatalarına açık|FLOAT / DOUBLE gibi|
|Kullanım Alanı|Standart ondalıklı sayılar|FLOAT / DOUBLE kullanıldığı her yer|
|Hassas Finansal İşlem|❌ Önerilmez (Decimal128 tercih edilir)|❌ FLOAT / DOUBLE aynı riskte|

---

## 🌟 **Avantaj**

✅ Büyük ve küçük ondalıklı değerleri kolayca tutabilir  
✅ Hızlı aritmetik işlem desteği  
✅ Aggregation pipeline ile kolay kullanılabilir

---

## 🚩 **Dezavantaj**

❌ Finansal hassasiyet gereken işlemler için riskli olabilir (ör. para birimi)  
❌ Çok büyük belgelerde Double boyutu yer kaplayabilir (32 bit int’e göre)