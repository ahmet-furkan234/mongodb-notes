
> Aggregation pipeline içinde verileri **belirli bir alan veya alanlara göre sıralamak** için kullanılır.
- SQL'deki `ORDER BY` ifadesinin MongoDB karşılığıdır.
- Sıralama yönü:
    - `1` → Artan sıralama (ascending)
    - `-1` → Azalan sıralama (descending)

---

## 📁 Örnek Veri: `orders`

```js
db.orders.insertMany([
  { customer: "Ali",  product: "TV",    total: 1000, date: ISODate("2024-01-10") },
  { customer: "Ali",  product: "Phone", total: 500,  date: ISODate("2024-02-01") },
  { customer: "Ali",  product: "Laptop",total: 2000, date: ISODate("2024-03-01") },
  { customer: "Ayşe", product: "Tablet",total: 800,  date: ISODate("2024-01-05") },
  { customer: "Ayşe", product: "Phone", total: 600,  date: ISODate("2024-04-15") }
])
```

---

## 📌 1. Tüm siparişleri **toplam fiyata göre artan** sırala

```js
db.orders.aggregate([
  { $sort: { total: 1 } }
])
```

> ✅ Küçükten büyüğe sıralama

---

## 📌 2. Tüm siparişleri **toplam fiyata göre azalan** sırala

```js
db.orders.aggregate([
  { $sort: { total: -1 } }
])
```

> ✅ Büyükten küçüğe sıralama

---

## 📌 3. Çoklu alanlara göre sıralama

```js
db.orders.aggregate([
  { $sort: { customer: 1, date: -1 } }
])
```

> ✅ Önce müşteri adına göre A–Z sıralar, sonra aynı müşteriye ait verileri tarihe göre yeni–eski sıralar.

---

## 📌 4. Sadece sıralı sonuçların ilk X tanesini almak

> İlk 2 en pahalı siparişi getir:

```js
db.orders.aggregate([
  { $sort: { total: -1 } },
  { $limit: 2 }
])
```

---

## 📌 5. `$group` öncesinde `$sort` kullanımı

> `customer`'a göre gruplanacak, ama **tarih sırasına göre** ilk ve son sipariş alınacak:

```js
db.orders.aggregate([
  { $sort: { customer: 1, date: 1 } },
  {
    $group: {
      _id: "$customer",
      firstProduct: { $first: "$product" },
      lastProduct: { $last: "$product" }
    }
  }
])
```

---

## 📌 6. `$match` ile birlikte

> Sadece `total > 700` olan siparişleri **en yeni tarihe göre sırala**

```js
db.orders.aggregate([
  { $match: { total: { $gt: 700 } } },
  { $sort: { date: -1 } }
])
```

---

## 📌 7. `$project` ile sıralama sonrası seçme

```js
db.orders.aggregate([
  { $sort: { total: -1 } },
  { $project: { _id: 0, customer: 1, product: 1, total: 1 } }
])
```

---

## 📌 8. `$sort` sonrası `$group` kullanımı ile kombinasyon

```js
db.orders.aggregate([
  { $sort: { date: 1 } },
  {
    $group: {
      _id: "$customer",
      firstDate: { $first: "$date" },
      lastDate: { $last: "$date" }
    }
  }
])
```

---

## 💡 Performans Notları

- **`$sort` en başta** kullanılacaksa, mutlaka **index** oluştur:

```js
db.orders.createIndex({ total: 1 })
```

- Aksi halde sıralama RAM’de yapılır ve **yavaşlar**.
- `allowDiskUse: true` ile büyük veri setlerinde disk kullanabilirsin:

```js
db.orders.aggregate([ { $sort: { total: -1 } } ], { allowDiskUse: true })
```

---

## 🎯 Kısa Karşılaştırma

|SQL|MongoDB|
|---|---|
|`ORDER BY col ASC`|`{ $sort: { col: 1 } }`|
|`ORDER BY col DESC`|`{ $sort: { col: -1 } }`|
|Çoklu sıralama|`{ $sort: { col1: 1, col2: -1 } }`|

---

## ✅ Özet

|Özellik|`$sort`|
|---|---|
|Artan sıralama (`ASC`)|`1`|
|Azalan sıralama (`DESC`)|`-1`|
|Çoklu sıralama destekler|✅|
|Performansa etki eder (index şart)|✅|
|Diğer aşamalarla kullanılabilir|✅ (`$match`, `$limit`, `$group`, vb.)|
