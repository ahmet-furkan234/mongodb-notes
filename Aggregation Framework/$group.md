
- SQL'deki `GROUP BY` işleminin MongoDB karşılığıdır.
- Verileri belirli bir **anahtar** (_id) etrafında gruplayarak istatistiksel işlemler yapılmasını sağlar.

---

## 🔧 Söz Dizimi

```js
{
  $group: {
    _id: "$alan",          // Grup anahtarı
    toplam: { $sum: "$fiyat" },  // Gruplanan alandaki işlemler
    sayi: { $sum: 1 },           // Her belge için 1 ekleyerek sayma
    ortalama: { $avg: "$puan" }
  }
}
```

---

## 📁 Örnek Veri Seti: `orders`

```js
db.orders.insertMany([
  { customer: "Ali", total: 100, status: "completed" },
  { customer: "Ali", total: 200, status: "pending" },
  { customer: "Ayşe", total: 150, status: "completed" },
  { customer: "John", total: 300, status: "completed" },
  { customer: "Ali", total: 50,  status: "completed" },
  { customer: "Ayşe", total: 80,  status: "pending" }
])
```

---

## 📌 1. Müşteri bazlı toplam harcama

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      totalSpent: { $sum: "$total" }
    }
  }
])
```

> ✅ Her müşterinin toplam harcaması döner.

---

## 📌 2. Müşteri sipariş sayısı

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      orderCount: { $sum: 1 }
    }
  }
])
```

> ✅ Her müşteri için toplam sipariş sayısı

---

## 📌 3. Sipariş durumlarına göre ortalama tutar

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$status",
      avgTotal: { $avg: "$total" }
    }
  }
])
```

> ✅ `completed` ve `pending` durumlarına göre ortalama sipariş tutarı

---

## 📌 4. Birden fazla işlem aynı anda

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      total: { $sum: "$total" },
      count: { $sum: 1 },
      average: { $avg: "$total" },
      min: { $min: "$total" },
      max: { $max: "$total" }
    }
  }
])
```

> ✅ Tüm istatistikler birlikte hesaplanabilir.

---

## 📌 5. `$match` + `$group` kombinasyonu (çok yaygın!)

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  {
    $group: {
      _id: "$customer",
      total: { $sum: "$total" }
    }
  }
])
```

> ✅ Yalnızca `completed` olan siparişleri gruplar.

---

## 📌 6. Gruplanan değerleri dizi olarak toplama

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      allAmounts: { $push: "$total" }
    }
  }
])
```

> ✅ Her müşterinin yaptığı ödemeleri liste şeklinde getirir.

---

## 📌 7. Benzersiz değerleri diziye ekleme (`$addToSet`)

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      statuses: { $addToSet: "$status" }
    }
  }
])
```

> ✅ Aynı müşteri aynı statüyü birden çok kez geçse bile yalnızca 1 kez görünür (tekilleştirir).

---

## 📌 8. `_id: null` kullanımı → tüm koleksiyon için özet

```js
db.orders.aggregate([
  {
    $group: {
      _id: null,
      totalRevenue: { $sum: "$total" },
      orderCount: { $sum: 1 }
    }
  }
])
```

> ✅ Bütün siparişlerin toplamı ve sayısı

---

## 🧠 Önemli Not: `_id` sadece bir alan olmak zorunda değil

```js
$group: {
  _id: { customer: "$customer", status: "$status" },
  total: { $sum: "$total" }
}
```

> ✅ Hem müşteriye hem de sipariş durumuna göre gruplanır (çoklu grup anahtarı)

---

## 🔬 `find()` vs `$group` Farkı

|Özellik|`find()`|`$group`|
|---|---|---|
|Gruplama|❌ Yapamaz|✅ İstatistik üretir|
|Toplama, ortalama|❌ Yapamaz|✅ $sum, $avg, $min, $max|
|Dizilere toplama|❌ Yapamaz|✅ $push, $addToSet|

---

## 🔧 Desteklenen `$group` Operatörleri

|Operatör|Açıklama|
|---|---|
|`$sum`|Toplam (belirli alan ya da 1)|
|`$avg`|Ortalama|
|`$min`|Minimum değer|
|`$max`|Maksimum değer|
|`$push`|Tüm değerleri diziye ekler|
|`$addToSet`|Benzersiz değerleri diziye ekler|
|`$first`|İlk belge değeri|
|`$last`|Son belge değeri|

---

## 🎯 Özet

- **`$group`**, verileri bir anahtara göre gruplayarak **sayısal ve mantıksal özetler** çıkarmanı sağlar.
    
- En çok kullanılan aggregation aşamalarındandır.
    
- `$match` ile birlikte kullanmak performans açısından çok önemlidir.
    

---

Hazırsan şimdi sıradaki adım olan **`$sort`** ile sonuçları sıralama işlemlerine geçebiliriz, ya da `$push`, `$addToSet`, `$first` gibi ileri `$group` tekniklerini detaylı işleyebiliriz. Ne dersin?