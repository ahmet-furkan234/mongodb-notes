
- Belge üzerindeki alanları **seçmek**, **göstermek**, **gizlemek** veya **dönüştürmek** için kullanılır.
- SQL'deki `SELECT` işlemine benzer ama çok daha güçlüdür.

---

## 📦 Temel Kullanımı

```js
db.collection.aggregate([
  {
    $project: {
      alan1: 1,   // göster
      alan2: 0    // gizle
    }
  }
])
```

> Varsayılan olarak `_id` gösterilir. Onu da `0` ile gizleyebilirsin.

---

## 🧪 Örnek Veri Seti: `users`

```js
db.users.insertMany([
  { name: "Ali", age: 25, country: "Turkey", email: "ali@gmail.com" },
  { name: "Ayşe", age: 32, country: "Turkey", email: "ayse@hotmail.com" },
  { name: "John", age: 40, country: "USA", email: "john@yahoo.com" }
])
```

---

## 📌 1. Belirli alanları seçme (projection)

```js
db.users.aggregate([
  {
    $project: {
      name: 1,
      email: 1,
      _id: 0
    }
  }
])
```

> Sadece `name` ve `email` alanlarını gösterir. `_id` gizlenmiştir.

---

## 📌 2. Yeni alan oluşturma (`alias` gibi)

```js
db.users.aggregate([
  {
    $project: {
      fullName: "$name",
      countryName: "$country",
      _id: 0
    }
  }
])
```

> `name` → `fullName`, `country` → `countryName` şeklinde yeniden adlandırılmıştır.

---

## 📌 3. Matematiksel işlem yapma

```js
db.users.aggregate([
  {
    $project: {
      name: 1,
      birthYear: { $subtract: [2025, "$age"] },
      _id: 0
    }
  }
])
```

> `birthYear` adında yeni bir alan oluşur.

---

## 📌 4. Metin birleştirme (concatenation)

```js
db.users.aggregate([
  {
    $project: {
      name: 1,
      contact: { $concat: ["Email: ", "$email"] },
      _id: 0
    }
  }
])
```

---

## 📌 5. Koşullu ifade (`$cond`)

```js
db.users.aggregate([
  {
    $project: {
      name: 1,
      status: {
        $cond: {
          if: { $gte: ["$age", 30] },
          then: "Adult",
          else: "Young"
        }
      },
      _id: 0
    }
  }
])
```

> 30 yaş ve üstü → "Adult", aksi halde "Young"

---

## 📌 6. `$toUpper`, `$toLower` — Harf dönüşümü

```js
db.users.aggregate([
  {
    $project: {
      nameUpper: { $toUpper: "$name" },
      nameLower: { $toLower: "$name" },
      _id: 0
    }
  }
])
```

---

## 📌 7. Alanları gizleme

```js
db.users.aggregate([
  {
    $project: {
      email: 0
    }
  }
])
```

> Tüm alanlar döner ama `email` gizlenir (varsayılan olarak diğerleri gelir).

---

## 📌 8. `$project` ile iç içe alanlar

Örnek veri:

```js
{
  name: "Ali",
  address: { city: "Istanbul", zip: "34000" }
}
```

### Sadece city alanını almak:

```js
$project: {
  "address.city": 1,
  _id: 0
}
```

---

## 🧠 `$project` vs `$addFields`

|Özellik|`$project`|`$addFields`|
|---|---|---|
|Alanları seçer|✅|❌|
|Alan ekler|✅|✅|
|Alan gizler|✅|❌|
|Sadece yeni alan|Hayır, tüm belgeyi kontrol eder|Evet, sadece yeni eklemeye odaklı|

---

## 🎓 Gerçek Hayat Senaryosu

> "Her kullanıcının adını büyük harfli şekilde döndür ve yaşa göre yetişkin/çocuk sınıflandırması yap."

```js
db.users.aggregate([
  {
    $project: {
      name: { $toUpper: "$name" },
      ageGroup: {
        $cond: {
          if: { $lt: ["$age", 18] },
          then: "Minor",
          else: "Adult"
        }
      },
      _id: 0
    }
  }
])
```

---

## 🎯 Özet

|Yeteneği|`$project` ile yapılabilir mi?|
|---|---|
|Alan gizleme|✅ `alan: 0`|
|Alan seçme|✅ `alan: 1`|
|Yeni alan oluşturma|✅ `alias: "$field"`|
|Matematik işlemleri|✅ `$add`, `$subtract`, vs.|
|Metin işlemleri|✅ `$concat`, `$toUpper`|
|Koşullu mantık|✅ `$cond`|
