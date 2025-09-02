
> `$count`, filtrelenmiş veya işlenmiş belgelerin **sayısını verir** ve sonucu bir isimle döndürür.

📌 SQL karşılığı:

```sql
SELECT COUNT(*) AS total FROM ...
```

---

## 🧬 Temel Kullanım

### 📌 Söz dizimi:

```js
{ $count: "fieldName" }
```

- `"fieldName"` → Sayının döneceği alanın adıdır.

---

## 📁 Örnek Veri: `orders`

```js
db.orders.insertMany([
  { customer: "Ali",  product: "TV",    total: 1000 },
  { customer: "Ali",  product: "Phone", total: 500 },
  { customer: "Ali",  product: "TV",    total: 1200 },
  { customer: "Ayşe", product: "Tablet",total: 800 },
  { customer: "Ayşe", product: "Phone", total: 600 }
])
```

---

## 📌 1. Tüm belge sayısını getir

```js
db.orders.aggregate([
  { $count: "orderCount" }
])
```

📤 Çıktı:

```json
{ "orderCount": 5 }
```

---

## 📌 2. Belirli koşula uyan belge sayısını getir

> Toplamı 1000 TL’den büyük olan siparişlerin sayısı:

```js
db.orders.aggregate([
  { $match: { total: { $gt: 1000 } } },
  { $count: "expensiveOrderCount" }
])
```

📤 Çıktı:

```json
{ "expensiveOrderCount": 1 }
```

---

## 📌 3. `distinct` gibi farklı müşteri sayısı? (❌ `$count` yetmez)

```js
db.orders.aggregate([
  { $group: { _id: "$customer" } },
  { $count: "uniqueCustomerCount" }
])
```

> ✅ Bu şekilde **farklı müşteri** sayısını elde edebilirsin.  
> `$count` **tek başına** farklı değer saymaz, ama `$group` sonrası kullanımı destekler.

---

## 📌 4. Belgeyi saymak yerine `$group` + `$sum: 1` ile aynısı:

```js
db.orders.aggregate([
  {
    $group: {
      _id: null,
      count: { $sum: 1 }
    }
  }
])
```

> Bu da toplam belge sayısını verir ama `$count` kadar kısa ve temiz değildir.

---

## 🧠 Teknik Notlar

|Özellik|Detay|
|---|---|
|Yalnızca **son aşama** olabilir|✅|
|Alan adı özelleştirilebilir|✅|
|`$group` alternatifi midir?|Evet (kısa versiyonudur)|
|`distinct` gibi çalışır mı?|❌ (sadece belge sayar, benzersiz saymaz)|

---

## 🎯 Gerçek Senaryo

> "Bu ay toplam kaç işlem yapılmış?"

```js
db.transactions.aggregate([
  { $match: { date: { $gte: ISODate("2024-07-01") } } },
  { $count: "julyTransactionCount" }
])
```

---

## ✅ Özet

|Amaç|Önerilen Yol|
|---|---|
|Tüm belge sayısı|`$count: "total"`|
|Şartlı sayım|`$match` + `$count`|
|Farklı değerleri saymak|`$group` + `$count`|
|Alternatif yol|`$group: { $sum: 1 }`|
