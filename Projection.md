
MongoDB’de **projection**, `find()` gibi sorgularda **yalnızca belirli alanları getirmeni veya bazı alanları gizlemeni** sağlar.

> 🔍 SQL'deki `SELECT name, age FROM users` gibi düşünebilirsin.

---

## ✅ Temel Kullanım:

```js
db.collection.find({ filtre }, { alan1: 1, alan2: 1, _id: 0 })
```

- `1` → alanı dahil et
- `0` → alanı dışla
- `_id` → varsayılan olarak her zaman gelir, istersen `0` diyerek hariç tutabilirsin

---

## 🔍 Örnekler:

### 1️⃣ Sadece `name` ve `age` alanlarını getir:

```js
db.users.find({}, { name: 1, age: 1, _id: 0 })
```

### 2️⃣ `password` ve `email` hariç her alanı getir:

```js
db.users.find({}, { password: 0, email: 0 })
```

> ⚠️ Dikkat: **Aynı anda hem dahil hem hariç yapılamaz.**  
> Sadece `_id` istisna olarak ayrı şekilde dışlanabilir.

---

## 🧠 Kural:

|Projection Türü|Açıklama|
|---|---|
|**Dahil Etme (`1`)**|Belirli alanları getirir|
|**Dışlama (`0`)**|Belirli alanları getirmez|
|**Karışık Kullanma ❌**|`name: 1, password: 0` → **hata verir**|

---

## 🧪 Gerçekçi Örnekler:

### 🔐 Giriş ekranında yalnızca `username` ve `email` getir:

```js
db.users.find({}, { username: 1, email: 1, _id: 0 })
```

---

### 🔒 Admin hariç `password` alanını gösterme:

```js
db.users.find({}, { password: 0 })
```

---

### 📦 Ürün listesinde sadece ad, fiyat ve kategori:

```js
db.products.find(
  { inStock: true },
  { name: 1, price: 1, category: 1, _id: 0 }
)
```

---

## 📌 `_id` Alanı Özel

- Projection kullanmasan bile her `find()` sonucunda `_id` gelir.
- Eğer istemiyorsan:

```js
{ _id: 0 }
```

---

## 🧠 Dizi Elemanlarıyla Projection (Positional Operator)

### Yalnızca dizi içinden eşleşen elemanı döndür:

```js
db.comments.find(
  { "replies.author": "TkMatE" },
  { "replies.$": 1 }
)
```

> `$` → Eşleşen ilk elemanı getirir

---

## 🧮 Aggregation'da `$project` Kullanımı

```js
db.users.aggregate([
  { $project: { name: 1, age: 1, isAdult: { $gte: ["$age", 18] } } }
])
```

> Sadece `name`, `age` döner ve ayrıca yeni bir alan `isAdult` oluşturulur

---

## 🎯 Kullanım Özet Tablosu

|Projection Yapısı|Anlamı|
|---|---|
|`{ name: 1, age: 1 }`|Sadece `name` ve `age` döner|
|`{ password: 0 }`|`password` dışında tüm alanlar|
|`{ _id: 0 }`|`_id` alanını hariç tut|
|`{ replies.$: 1 }`|Dizi içinde sadece ilk eşleşeni|
|`{ $project: { age: 1, adult: { $gte: ["$age", 18] } } }`|Aggregation projection örneği|

---

## 🛠 Pratik: Sadece isim ve doğum yılı olan kullanıcılar

```js
db.users.find({}, { name: 1, birthYear: 1, _id: 0 })
```

---

## 🔚 Özet:

|Özellik|Açıklama|
|---|---|
|`1`|Alanı dahil et|
|`0`|Alanı dışla|
|`_id`|Varsayılan olarak gelir|
|Karışık Kullanım|Aynı anda hem `1` hem `0` yasak|
|Dizi `$`|İlk eşleşen öğeyi getirir|
