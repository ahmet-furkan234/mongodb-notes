
> `$unwind`, bir belgedeki **dizi (array)** alanını, her elemanı için **ayrı bir belgeye çevirir**.
> SQL karşılığı: `CROSS APPLY` / `UNNEST` gibi düşünülebilir.

---

## 📁 Örnek Veri: `orders`

```js
db.orders.insertMany([
  {
    _id: 1,
    customer: "Ali",
    items: ["TV", "Phone", "Laptop"]
  },
  {
    _id: 2,
    customer: "Ayşe",
    items: ["Tablet", "Phone"]
  }
])
```

---

## 📌 1. Temel `$unwind` Kullanımı

```js
db.orders.aggregate([
  { $unwind: "$items" }
])
```

📤 Çıktı:

```json
{ _id: 1, customer: "Ali",  items: "TV" }
{ _id: 1, customer: "Ali",  items: "Phone" }
{ _id: 1, customer: "Ali",  items: "Laptop" }
{ _id: 2, customer: "Ayşe", items: "Tablet" }
{ _id: 2, customer: "Ayşe", items: "Phone" }
```

> ✅ Her **dizi elemanı ayrı belge** haline gelir.

---

## 📌 2. Obje dizisi ile kullanımı

```js
db.orders2.insertMany([
  {
    _id: 1,
    customer: "Ali",
    items: [
      { product: "TV", qty: 1 },
      { product: "Phone", qty: 2 }
    ]
  }
])
```

```js
db.orders2.aggregate([
  { $unwind: "$items" }
])
```

📤 Çıktı:

```json
{ _id: 1, customer: "Ali", items: { product: "TV", qty: 1 } }
{ _id: 1, customer: "Ali", items: { product: "Phone", qty: 2 } }
```

---

## 📌 3. `$unwind` sonrası `$group` ile ürün adedi toplama

```js
db.orders2.aggregate([
  { $unwind: "$items" },
  {
    $group: {
      _id: "$items.product",
      totalQty: { $sum: "$items.qty" }
    }
  }
])
```

📤 Çıktı:

```json
{ _id: "TV", totalQty: 1 }
{ _id: "Phone", totalQty: 2 }
```

---

## 📌 4. `preserveNullAndEmptyArrays: true`

Dizisi **olmayan** veya **boş** olan belgelerin de gelmesini istersen:

```js
db.orders3.insertMany([
  { _id: 1, name: "Ali", hobbies: ["fishing", "cycling"] },
  { _id: 2, name: "Ayşe", hobbies: [] },
  { _id: 3, name: "Mehmet" }
])
```

```js
db.orders3.aggregate([
  {
    $unwind: {
      path: "$hobbies",
      preserveNullAndEmptyArrays: true
    }
  }
])
```

📤 Çıktı:

```json
{ _id: 1, name: "Ali", hobbies: "fishing" }
{ _id: 1, name: "Ali", hobbies: "cycling" }
{ _id: 2, name: "Ayşe", hobbies: null }
{ _id: 3, name: "Mehmet", hobbies: null }
```

> ✅ Varsayılan davranışta 2 ve 3 no’lu kayıtlar gelmezdi. `preserveNullAndEmptyArrays: true` sayesinde gelirler.

---

## 📌 5. `includeArrayIndex` ile orijinal index bilgisini almak

```js
db.orders.aggregate([
  {
    $unwind: {
      path: "$items",
      includeArrayIndex: "itemIndex"
    }
  }
])
```

📤 Çıktı:

```json
{ _id: 1, customer: "Ali", items: "TV", itemIndex: 0 }
{ _id: 1, customer: "Ali", items: "Phone", itemIndex: 1 }
{ _id: 1, customer: "Ali", items: "Laptop", itemIndex: 2 }
```

> ✅ Hangi dizi elemanının kaçıncı sırada olduğunu öğrenmiş oluruz.

---

## 🧠 Teknik Notlar

|Özellik|Destekleniyor|
|---|---|
|Dizi olmayan belgeyi atla|✅ (default)|
|Boş diziye sahip belgeyi atla|✅ (default)|
|Dizi yoksa belgeyi koruma|✅ (opsiyonel)|
|Index bilgisi ekleme|✅ (isteğe bağlı)|

---

## 📊 `$unwind` ile İleri Kullanımlar

### Örnek: Öğrencilerin katıldığı kulüplerin listesini açmak

```js
db.students.insertMany([
  { name: "Ali", clubs: ["Basketball", "Chess"] },
  { name: "Ayşe", clubs: ["Math", "Chess"] }
])
```

```js
db.students.aggregate([
  { $unwind: "$clubs" },
  {
    $group: {
      _id: "$clubs",
      members: { $addToSet: "$name" }
    }
  }
])
```

📤 Çıktı:

```json
{ _id: "Basketball", members: ["Ali"] }
{ _id: "Chess", members: ["Ali", "Ayşe"] }
{ _id: "Math", members: ["Ayşe"] }
```

---

## ✅ Özet

|Amaç|`$unwind` ile yapılabilir mi?|
|---|---|
|Dizideki her elemanı ayrı belge yapmak|✅|
|Dizi olmayan kayıtları dışarıda bırakmak|✅ (default)|
|Dizi olmayanları tutmak|✅ `preserveNullAndEmptyArrays` ile|
|Dizi index'ini belgeye dahil etmek|✅ `includeArrayIndex` ile|
