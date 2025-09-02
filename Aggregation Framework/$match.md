
- Aggregation pipeline’ın en başında gelen filtredir.
- SQL'deki `WHERE` koşuluna denk gelir.
- Normal `find()` sorgularıyla aynı sözdizimini kullanır ama **aggregation içinde daha güçlüdür.**

---

## 🔧 Temel Söz Dizimi

```js
db.collection.aggregate([
  { $match: { field: value } }
])
```

---

## 📁 Örnek Veri Seti: `users`

```js
db.users.insertMany([
  { name: "Ali", age: 25, country: "Turkey", isActive: true },
  { name: "Ayşe", age: 32, country: "Turkey", isActive: false },
  { name: "John", age: 40, country: "USA", isActive: true },
  { name: "Jane", age: 29, country: "Germany", isActive: true },
  { name: "Mehmet", age: 19, country: "Turkey", isActive: true },
  { name: "Emma", age: 35, country: "UK", isActive: false }
])
```

---

## 📌 1. Basit Koşul: Türkiye'deki kullanıcıları getir

```js
db.users.aggregate([
  { $match: { country: "Turkey" } }
])
```

> 👉 3 belge döner: Ali, Ayşe, Mehmet

---

## 📌 2. Çoklu Koşul (`AND`): Türkiye'de ve aktif olanlar

```js
db.users.aggregate([
  { $match: { country: "Turkey", isActive: true } }
])
```

> 👉 Ali ve Mehmet

---

## 📌 3. `OR` Koşulu (`$or`)

```js
db.users.aggregate([
  {
    $match: {
      $or: [
        { country: "Turkey" },
        { age: { $lt: 30 } }
      ]
    }
  }
])
```

> 👉 Ali, Ayşe, Mehmet, Jane → (Türkiye'de olanlar veya yaşı 30'dan küçük olanlar)

---

## 📌 4. Karşılaştırma Operatörleri

|Operatör|Açıklama|Örnek|
|---|---|---|
|`$eq`|Eşittir|`{ age: { $eq: 25 } }`|
|`$ne`|Eşit değildir|`{ age: { $ne: 25 } }`|
|`$gt`|Büyüktür|`{ age: { $gt: 30 } }`|
|`$gte`|Büyük eşit|`{ age: { $gte: 30 } }`|
|`$lt`|Küçüktür|`{ age: { $lt: 30 } }`|
|`$lte`|Küçük eşit|`{ age: { $lte: 30 } }`|
|`$in`|İçinde|`{ country: { $in: ["Turkey", "USA"] } }`|
|`$nin`|Dışında|`{ country: { $nin: ["UK", "Germany"] } }`|

---

## 📌 5. `in` Kullanımı

```js
db.users.aggregate([
  { $match: { country: { $in: ["Turkey", "USA"] } } }
])
```

> 👉 Ali, Ayşe, Mehmet, John

---

## 📌 6. `not` ve `ne` ile filtreleme

```js
db.users.aggregate([
  { $match: { isActive: { $ne: true } } }
])
```

> 👉 Ayşe ve Emma

---

## 📌 7. `and` ile kompleks koşullar

```js
db.users.aggregate([
  {
    $match: {
      $and: [
        { country: "Turkey" },
        { age: { $gte: 30 } }
      ]
    }
  }
])
```

> 👉 Ayşe

---

## 📌 8. `regex` ile arama (metin içinde)

```js
db.users.aggregate([
  {
    $match: {
      name: { $regex: /^A/i }
    }
  }
])
```

> 👉 Ali, Ayşe → (A harfiyle başlayan adlar)

---

## 📌 9. Boolean koşullar

```js
db.users.aggregate([
  { $match: { isActive: true } }
])
```

> 👉 Ali, John, Jane, Mehmet

---

## 📌 10. Dizi veri tipi olan koleksiyonlarda `$elemMatch` (örnek veriyle göstereceğim)

```js
db.students.insertMany([
  { name: "Zeynep", grades: [85, 92, 77] },
  { name: "Deniz", grades: [45, 55, 60] },
  { name: "Kaan", grades: [90, 91, 93] }
])
```

```js
db.students.aggregate([
  {
    $match: {
      grades: { $elemMatch: { $gt: 90 } }
    }
  }
])
```

> 👉 Kaan (çünkü notlardan biri 90’dan büyük)

---

## 📌 11. Tarihe göre filtreleme

```js
db.logs.insertMany([
  { user: "Ali", createdAt: new Date("2024-01-01") },
  { user: "Ayşe", createdAt: new Date("2025-01-01") },
  { user: "Mehmet", createdAt: new Date("2025-07-01") }
])
```

```js
db.logs.aggregate([
  {
    $match: {
      createdAt: { $gte: new Date("2025-01-01") }
    }
  }
])
```

> 👉 Ayşe, Mehmet

---

## 💡 İpucu: `$match` her zaman pipeline’ın en başında yer almalı

Çünkü:

- En az veriyle devam eder → Daha hızlı çalışır
- Pipeline’a yük bindirmez

---

## 🚀 Gerçek Hayat Senaryosu

### Soru:

> "Yaşı 30'dan büyük olan Türkiye'deki aktif kullanıcıların e-posta listesini getir."

```js
db.users.aggregate([
  { $match: { country: "Turkey", age: { $gt: 30 }, isActive: true } },
  { $project: { name: 1, _id: 0 } }
])
```

---

## 🔍 `explain()` ile analiz

```js
db.users.aggregate([
  { $match: { isActive: true } }
]).explain("executionStats")
```

> Index kullanımı, belge taraması vs. gösterir.

---

## 🧠 Kapanış

`$match` aggregation pipeline’ın en kritik parçasıdır çünkü:

- Diğer aşamaların iş yükünü azaltır
- En iyi performans için **başta yer almalıdır**
- `find()` ile aynı sözdizimini kullanır ama çok daha esnektir
