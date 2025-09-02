
> `$lookup`, bir koleksiyonu başka bir koleksiyonla birleştirir (JOIN).  
> İki belge setini, ortak bir alan üzerinden eşleştirir.

> SQL eşleniği:

```sql
SELECT * FROM orders
JOIN users ON orders.userId = users._id
```

---

## 🧬 Temel Söz Dizimi

```js
{
  $lookup: {
    from: "targetCollection",
    localField: "localKey",
    foreignField: "foreignKey",
    as: "resultField"
  }
}
```

|Alan|Açıklama|
|---|---|
|`from`|JOIN yapılacak koleksiyon adı|
|`localField`|Ana koleksiyondaki eşleşecek alan|
|`foreignField`|`from` koleksiyonundaki eşleşecek alan|
|`as`|Eşleşen kayıtların atanacağı dizi alan adı|

---

## 📁 Örnek Veri Seti

### `orders` koleksiyonu:

```js
db.orders.insertMany([
  { _id: 1, userId: 101, product: "TV", total: 1000 },
  { _id: 2, userId: 102, product: "Phone", total: 500 },
  { _id: 3, userId: 103, product: "Laptop", total: 2000 }
])
```

### `users` koleksiyonu:

```js
db.users.insertMany([
  { _id: 101, name: "Ali" },
  { _id: 102, name: "Ayşe" },
  { _id: 104, name: "Mehmet" }
])
```

---

## 📌 1. Basit `$lookup`: Kullanıcı bilgilerini siparişlere ekle

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "userInfo"
    }
  }
])
```

📤 Çıktı:

```json
{
  _id: 1,
  userId: 101,
  product: "TV",
  total: 1000,
  userInfo: [{ _id: 101, name: "Ali" }]
}
```

> ✅ `userInfo` artık `orders` içine gömülü dizidir.

---

## 📌 2. `$unwind` ile kullanıcıyı dizi yerine obje yapmak

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "userInfo"
    }
  },
  { $unwind: "$userInfo" }
])
```

> ✅ Artık `userInfo.name` doğrudan erişilebilir.

---

## 📌 3. `$project` ile özelleştirme

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "userInfo"
    }
  },
  { $unwind: "$userInfo" },
  {
    $project: {
      _id: 0,
      product: 1,
      total: 1,
      customerName: "$userInfo.name"
    }
  }
])
```

📤 Çıktı:

```json
{ product: "TV", total: 1000, customerName: "Ali" }
{ product: "Phone", total: 500, customerName: "Ayşe" }
```

---

## 📌 4. Eşleşmeyen kullanıcılar ne olur?

- Eğer `orders.userId = 103` ama `users._id = 103` yoksa, `userInfo: []` olur.
- `unwind` yaparsan bu satır düşer (default `preserveNullAndEmptyArrays: false`).

---

## 📌 5. `$lookup` + `$match`: INNER JOIN mantığı

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "userInfo"
    }
  },
  { $unwind: "$userInfo" },
  { $match: { "userInfo.name": { $exists: true } } }
])
```

> ✅ Yalnızca eşleşen kayıtlar gelir (SQL’deki `INNER JOIN` gibi).

---

## 🧠 Teknik Notlar

|Özellik|Detay|
|---|---|
|JOIN tipi|LEFT OUTER JOIN (default)|
|INNER JOIN yapmak|`$unwind` + `$match` gerekir|
|Diziyi düzleştirme|`$unwind` kullanılır|
|Index önerisi|`foreignField`'e index önerilir (performans için)|

---

## 🎯 Gerçek Senaryo

> Ürün kategorileri ile ürünleri birleştir:

### Koleksiyonlar:

```js
products = [
  { name: "TV", categoryId: 1 },
  { name: "Phone", categoryId: 2 }
]

categories = [
  { _id: 1, name: "Electronics" },
  { _id: 2, name: "Mobile" }
]
```

### Query:

```js
db.products.aggregate([
  {
    $lookup: {
      from: "categories",
      localField: "categoryId",
      foreignField: "_id",
      as: "categoryInfo"
    }
  },
  { $unwind: "$categoryInfo" },
  {
    $project: {
      _id: 0,
      name: 1,
      category: "$categoryInfo.name"
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", category: "Electronics" }
{ name: "Phone", category: "Mobile" }
```

---

## ✅ Özet

|Özellik|Destekleniyor|
|---|---|
|Farklı koleksiyonla JOIN|✅ `$lookup`|
|Diziyi düzleştirme|✅ `$unwind`|
|Eşleşmeyeni hariç tutmak|✅ `$match`|
|Performans için index gerekir mi|Genellikle ✅|
