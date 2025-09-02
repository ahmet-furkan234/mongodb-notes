
- **Aggregation**, MongoDB'de birden fazla belgeyi alıp **filtreleme**, **gruplama**, **dönüştürme**, **sıralama**, **hesaplama** gibi işlemler yaparak veri üzerinde analizler üretmemizi sağlar.
- SQL'deki `GROUP BY`, `JOIN`, `HAVING`, `ORDER BY`, `SUM()`, `COUNT()` gibi işlemlerin karşılığıdır.

---

## 🎯 Kullanım Alanları

- Raporlama (istatistiksel özetler)
- Gruplama (ör. ülkeye göre kullanıcı sayısı)
- Filtreleme + Dönüştürme (ör. aktif kullanıcıları JSON olarak dönmek)
- Veritabanı içinde ETL işlemleri

---

## 🧱 Temel Yapı

```js
db.collection.aggregate([
  { /* aşama 1 */ },
  { /* aşama 2 */ },
  ...
])
```

> Her bir aşama, bir **pipeline stage (boru hattı adımı)**'dır.

---

## 🧩 Temel Aşamalar (Pipeline Stages)

| Aşama        | Açıklama                                   |
| ------------ | ------------------------------------------ |
| `$match`     | Belge filtresi (`WHERE`) gibi çalışır      |
| `$project`   | Belirli alanları seçme veya dönüştürme     |
| `$group`     | Gruplama + toplama işlemleri               |
| `$sort`      | Sıralama                                   |
| `$limit`     | Sonuç sayısını sınırlandırma               |
| `$skip`      | Belirli sayıda belgeyi atla                |
| `$lookup`    | Başka koleksiyonla join yapar              |
| `$unwind`    | Dizi alanlarını parçalar                   |
| `$count`     | Belge sayısını verir                       |
| `$addFields` | Belgeye yeni alan ekler                    |
| `$facet`     | Aynı anda birden fazla pipeline çalıştırır |
| `$merge`     | Başka koleksiyona yazma işlemi             |
| `$out`       | Sonucu yeni bir koleksiyona aktarır        |

---

## 🔎 1. `$match` — Filtreleme

```js
db.users.aggregate([
  { $match: { country: "Turkey", age: { $gte: 18 } } }
])
```

> `find()` gibi çalışır. Diğer aşamaları daraltmak için en başta kullanılır.

---

## 🧮 2. `$group` — Gruplama ve Toplama

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
      totalAmount: { $sum: "$amount" },
      orderCount: { $sum: 1 }
    }
  }
])
```

> SQL eşdeğeri:

```sql
SELECT customerId, SUM(amount), COUNT(*) FROM orders GROUP BY customerId;
```

### 🎯 Diğer `$group` operatörleri:

| Operatör    | Açıklama                     |
| ----------- | ---------------------------- |
| `$sum`      | Toplam                       |
| `$avg`      | Ortalama                     |
| `$min`      | Minimum değer                |
| `$max`      | Maksimum değer               |
| `$first`    | İlk değer (sıralamaya göre)  |
| `$last`     | Son değer (sıralamaya göre)  |
| `$push`     | Değerleri diziye ekler       |
| `$addToSet` | Tekil değerleri diziye ekler |

---

## 🧪 3. `$project` — Alan Seçme / Dönüştürme

```js
db.users.aggregate([
  {
    $project: {
      fullName: { $concat: ["$firstName", " ", "$lastName"] },
      email: 1,
      ageCategory: {
        $cond: { if: { $gte: ["$age", 18] }, then: "adult", else: "minor" }
      }
    }
  }
])
```

> Yeni alan üretme, eskiyi gizleme ve mantıksal dönüşümler için kullanılır.

---

## 🔃 4. `$sort`, `$limit`, `$skip`

```js
db.products.aggregate([
  { $sort: { price: -1 } },
  { $skip: 10 },
  { $limit: 5 }
])
```

> Ürünleri fiyata göre azalan sıralar, ilk 10’u atlar ve sonraki 5 ürünü döner.

---

## 🔄 5. `$unwind` — Dizi Parçalama

```js
db.orders.aggregate([
  { $unwind: "$items" },
  { $group: { _id: "$items.name", total: { $sum: "$items.price" } } }
])
```

> Dizi alanlarını her bir eleman için ayrı belge yapar.

---

## 🔗 6. `$lookup` — Join İşlemi

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerInfo"
    }
  },
  { $unwind: "$customerInfo" }
])
```

> SQL karşılığı:

```sql
SELECT * FROM orders JOIN customers ON orders.customerId = customers._id
```

---

## 🎛️ 7. `$addFields` vs `$set`

```js
db.users.aggregate([
  { $addFields: { yearOfBirth: { $subtract: [2025, "$age"] } } }
])
```

> Yeni alan ekler veya var olanı değiştirir. `$set` ile eşdeğerdir.

---

## 🧠 8. `$facet` — Paralel Pipeline

```js
db.products.aggregate([
  {
    $facet: {
      expensive: [
        { $match: { price: { $gte: 1000 } } },
        { $count: "count" }
      ],
      cheap: [
        { $match: { price: { $lt: 1000 } } },
        { $count: "count" }
      ]
    }
  }
])
```

> Aynı koleksiyon için aynı anda 2 farklı filtreleme yapılır.

---

## 💾 9. `$out`, `$merge` — Sonuçları Koleksiyona Yazma

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$customerId", total: { $sum: "$amount" } } },
  { $out: "customerTotals" }
])
```

> `customerTotals` adlı yeni koleksiyon oluşur ve veriler oraya yazılır.

---

## 🧪 Kompleks Örnek: Müşteri Bazlı Sipariş Özeti

```js
db.orders.aggregate([
  { $match: { status: "delivered" } },
  {
    $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$total" },
      avgSpent: { $avg: "$total" },
      orderCount: { $sum: 1 }
    }
  },
  { $sort: { totalSpent: -1 } },
  { $limit: 5 }
])
```

> En çok harcayan 5 müşteriyi getirir.

---

## 🔍 `explain()` ile Aggregation Analizi

```js
db.orders.aggregate([
  { $match: { status: "pending" } }
]).explain("executionStats")
```

---

## 📘 Aggregation Expression Örnekleri

|Operatör|Açıklama|
|---|---|
|`$concat`|Metin birleştirme|
|`$toUpper`, `$toLower`|Harf dönüşümü|
|`$cond`|Koşullu ifade (if-then-else)|
|`$arrayElemAt`|Diziden belirli index’i alır|
|`$filter`|Dizi içinde filtreleme yapar|
|`$map`|Dizi elemanlarını dönüştürür|
|`$dateToString`|Tarihi string’e çevirir|

---

## ✅ Özet

|Aşama|Amaç|
|---|---|
|`$match`|Filtreleme|
|`$group`|Gruplama, toplama, analiz|
|`$project`|Alan seçimi, dönüşüm|
|`$unwind`|Dizi parçalama|
|`$lookup`|Join|
|`$sort`|Sıralama|
|`$limit`|Sınır|
|`$facet`|Paralel analiz|
|`$addFields`|Yeni alan ekleme|
|`$out`|Sonucu koleksiyona yazma|
