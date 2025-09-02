
> `$expr`, **belge içindeki alanları** veya **değişkenleri** birbiriyle karşılaştırmak için kullanılır.  
> Normal sorgularda alanlar sabit değerlere karşılaştırılırken, `$expr` sayesinde **dinamik** karşılaştırma yapılır.

> SQL'deki `WHERE columnA = columnB` ifadesine karşılık gelir.

---

## ✅ Neden Kullanılır?

|Durum|Gerekli mi?|
|---|---|
|Alan ile sabit karşılaştırma|Hayır|
|Alan ile alan karşılaştırması|✅ Evet|
|`$lookup` içinde `$let` ile değişken kullanımı|✅ Evet|
|Matematiksel işlemler (alanlar arasında)|✅ Evet|

---

## 🧬 Basit Örnek Veri: `sales`

```js
db.sales.insertMany([
  { item: "A", price: 100, discount: 100 },
  { item: "B", price: 200, discount: 50 },
  { item: "C", price: 300, discount: 100 }
])
```

---

## 📌 1. `$expr` olmadan — Sabit değer karşılaştırması

```js
db.sales.find({ price: 100 }) // price 100 olanlar
```

---

## 📌 2. `$expr` ile — Alanlar arası karşılaştırma

> 🔎 `price == discount` olanları getir:

```js
db.sales.find({
  $expr: { $eq: ["$price", "$discount"] }
})
```

📤 Çıktı:

```json
{ item: "A", price: 100, discount: 100 }
```

---

## 📌 3. `$expr` ile matematik işlemi

> 🔎 İndirimi %30’dan fazla olan ürünler

```js
db.sales.find({
  $expr: {
    $gt: [
      { $divide: ["$discount", "$price"] },
      0.3
    ]
  }
})
```

📤 Çıktı:

```json
{ item: "A", ... }
{ item: "C", ... }
```

---

## 📌 4. `$expr` + `$and`, `$or`

> 🔎 `price > discount` **ve** `price < 250`

```js
db.sales.find({
  $expr: {
    $and: [
      { $gt: ["$price", "$discount"] },
      { $lt: ["$price", 250] }
    ]
  }
})
```

---

## 📌 5. `$lookup` içinde `$expr` kullanımı

Daha önce yaptığımız gibi pipeline `$lookup`'ta:

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      let: { user_id: "$userId" },
      pipeline: [
        {
          $match: {
            $expr: {
              $eq: ["$_id", "$$user_id"]
            }
          }
        }
      ],
      as: "user"
    }
  }
])
```

> ✅ Buradaki `$expr` ile `$_id` ile dışarıdan gelen `$$user_id` değişkeni karşılaştırıldı.

---

## 📌 6. `$expr` ile `$cond` (Koşullu değer)

> 🔎 Her satıra indirimli fiyat hesapla

```js
db.sales.aggregate([
  {
    $project: {
      item: 1,
      price: 1,
      discount: 1,
      finalPrice: {
        $cond: {
          if: { $gt: ["$discount", 0] },
          then: { $subtract: ["$price", "$discount"] },
          else: "$price"
        }
      }
    }
  }
])
```

📤 Çıktı:

```json
{ item: "A", price: 100, discount: 100, finalPrice: 0 }
{ item: "B", price: 200, discount: 50, finalPrice: 150 }
```

---

## 🔬 Desteklenen Operatörler (Örnekleri `$expr` içinde kullanılır)

|Operatör|Açıklama|
|---|---|
|`$eq`|Eşittir|
|`$ne`|Eşit değildir|
|`$gt`|Büyüktür|
|`$gte`|Büyük eşittir|
|`$lt`|Küçüktür|
|`$lte`|Küçük eşittir|
|`$and`|Ve|
|`$or`|Veya|
|`$cond`|Koşul (if–then–else)|
|`$add`, `$subtract`, `$multiply`, `$divide`|Matematiksel işlemler|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|Alan–alan karşılaştırma|✅|
|`$lookup` içinde eşleştirme|✅|
|Matematik ve mantıksal kıyaslar|✅|
|`$match` içinde kullanımı|✅|
|`$project`, `$addFields`, `$cond` gibi yerlerde de kullanılabilir|✅|

---

Hazırsan şimdi istersen `$cond`, `$switch`, `$map`, `$reduce`, `$filter`, `$set`, `$addFields`, `$merge` gibi **gelişmiş hesaplama** operatörlerine geçebiliriz.

Hangisiyle devam edelim TkMatE?