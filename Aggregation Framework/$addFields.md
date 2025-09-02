
## 🧠 `$addFields` Nedir?

```js
{ $addFields: { <alanAdı>: <ifade>, ... } }
```

- Yeni bir alan ekler.
- Var olan bir alanı güncelleyebilir.
- Diğer alanlara dayanarak hesaplama yapabilir.

---

## 📌 Temel Kullanım Örnekleri

### 🎯 Örnek 1: Yeni Alan Ekleme

```js
db.sales.aggregate([
  {
    $addFields: {
      totalPrice: { $multiply: ["$price", "$quantity"] }
    }
  }
])
```

🧾 Çıktı:

```json
{
  "_id": 1,
  "price": 100,
  "quantity": 2,
  "totalPrice": 200
}
```

---

### 🎯 Örnek 2: Var Olan Alanı Güncelleme

```js
db.sales.aggregate([
  {
    $addFields: {
      price: { $add: ["$price", 10] }
    }
  }
])
```

📌 `price` alanını her belge için 10 artırır.

---

### 🎯 Örnek 3: Şartlı Alan Eklemek

```js
db.sales.aggregate([
  {
    $addFields: {
      isExpensive: { $cond: [{ $gt: ["$price", 1000] }, true, false] }
    }
  }
])
```

📌 `isExpensive` alanı eklenir: `true` veya `false`

---

### 🎯 Örnek 4: `$project` ile Farkı

|Özellik|`$addFields`|`$project`|
|---|---|---|
|Alan ekler|✅|✅ (ama tüm alanları yeniden tanımlamak gerekir)|
|Alan gizler|❌|✅|
|Kullanım amacı|Alan eklemek/güncellemek|Alan seçmek/gizlemek/dönüştürmek|

---

## 🧪 Daha Karmaşık Örnek

```js
db.students.aggregate([
  {
    $addFields: {
      average: { $avg: ["$midterm", "$final"] },
      status: {
        $cond: [
          { $gte: [ { $avg: ["$midterm", "$final"] }, 50 ] },
          "Passed",
          "Failed"
        ]
      }
    }
  }
])
```

🧾 Çıktı:

```json
{
  "_id": 1,
  "name": "Ahmet",
  "midterm": 60,
  "final": 40,
  "average": 50,
  "status": "Passed"
}
```

---

## ✅ Ne Zaman Kullanılır?

- Yeni alan türetmek istiyorsan
- Alan güncellemek istiyorsan
- Facet sonucu sadeleştiriyorsan (örneğin `$arrayElemAt` ile)
- `$project` yerine sadece küçük bir hesaplama gerekiyorsa


---

## 💡 Son Notlar

- `$addFields` aslında `$set` ile aynıdır (MongoDB 4.2+ itibariyle).
- Birden fazla alan aynı anda eklenebilir.
- Performans açısından hafiftir, sadece projeleme aşamasında çalışır.
