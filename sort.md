
- Belge sorgularını, **bir veya birden fazla alana göre artan (ASC) veya azalan (DESC)** şekilde sıralamak.

---

## ✅ Temel Kullanım:

```js
db.collection.find().sort({ alanAdi: 1 })     // Artan (ASC)
db.collection.find().sort({ alanAdi: -1 })    // Azalan (DESC)
```

---

## 🧠 Sıralama Yönü

|Değer|Anlamı|
|---|---|
|`1`|Artan sıralama|
|`-1`|Azalan sıralama|

---

## 🔍 Basit Örnekler

### 1️⃣ Yaşa göre artan sıralama:

```js
db.users.find().sort({ age: 1 })
```

### 2️⃣ Yaşa göre azalan sıralama:

```js
db.users.find().sort({ age: -1 })
```

### 3️⃣ İsme göre alfabetik (artan) sıralama:

```js
db.users.find().sort({ name: 1 })
```

---

## 🔁 Çoklu Alanla Sıralama

Birden fazla alanla sıralama yapılabilir. Öncelik sırası sıralamanın yazıldığı sıraya göredir.

### Örnek:

```js
db.users.find().sort({ age: 1, name: 1 })
```

> Önce `age`'e göre artan, yaşlar eşitse `name`'e göre artan sıralama yapılır.

---

## 🧩 Sıralama + Filtreleme + Projeksiyon Örneği

```js
db.products.find(
  { category: "electronics" },         // filtreleme
  { name: 1, price: 1, _id: 0 }        // sadece name ve price alanlarını getir
).sort({ price: -1 })                 // fiyata göre azalan sırala
```

---

## 🧠 Not: `sort()` bellekte çalışır ama büyük koleksiyonlarda **index** kullanılması önemlidir!

### 🔍 İyileştirme için index kullan:

```js
db.users.createIndex({ age: 1 })
```

---

## 🧪 Sıralama + Limit + Skip

```js
db.users.find().sort({ age: -1 }).skip(10).limit(5)
```

> Yaşça en büyüklerden başlayarak 10 kişiyi atla, sonra 5 kişi getir.  
> Bu, sayfalama (pagination) için yaygın bir kullanım şeklidir.

---

## 🧠 Dizi İçinde `sort()` Sıralama Yapamaz

```js
db.users.find({ hobbies: { $exists: true } }).sort({ hobbies: 1 }) // Geçersiz sıralama olabilir!
```

> Dizi içinde sıralama yapmak için veriyi dönüştürmen gerekebilir (aggregation ile).

---

## 🧪 Komple Örnek

```js
db.orders.find(
  { status: "completed" },
  { customer: 1, total: 1, date: 1, _id: 0 }
).sort({ date: -1, total: -1 })
```

> Siparişleri **en son tarihten en önceye** doğru ve toplam tutara göre sıralar.

---

## 🎯 Aggregation İçinde `sort`

Eğer sıralamayı daha gelişmiş sorgularda (gruplama vs.) kullanacaksan, aggregation kullanılır:

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $sort: { total: -1 } },
  { $limit: 10 }
])
```

---

## 📌 Özet

|Yapı|Anlamı|
|---|---|
|`{ alan: 1 }`|Artan sıralama|
|`{ alan: -1 }`|Azalan sıralama|
|`{ alan1: 1, alan2: -1 }`|Çok alanlı sıralama|
|`sort() + skip() + limit()`|Sayfalama (pagination)|
|`aggregation > $sort`|Gelişmiş sıralama|

---

## 🔧 Bonus: En yaşlı 3 kullanıcıyı getir

```js
db.users.find({}, { name: 1, age: 1, _id: 0 })
  .sort({ age: -1 })
  .limit(3)
```

> `age` alanına göre en büyük 3 kullanıcıyı getir.
