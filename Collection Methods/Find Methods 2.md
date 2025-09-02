
MongoDB'de `find()` metodunda kullanabileceğin temel koşullar şunlardır:

|Operatör|Açıklama|SQL Karşılığı|
|---|---|---|
|`{ alan: ... }`|Doğrudan eşleşme|`WHERE alan = ...`|
|`$eq`|Eşittir|`=`|
|`$ne`|Eşit değil|`!=` veya `<>`|
|`$gt`|Büyüktür|`>`|
|`$gte`|Büyük eşit|`>=`|
|`$lt`|Küçüktür|`<`|
|`$lte`|Küçük eşit|`<=`|
|`$in`|Belirli değerlerden biri|`IN (...)`|
|`$nin`|Belirli değerler dışındakiler|`NOT IN (...)`|
|`$and`|Tüm koşullar sağlanmalı|`AND`|
|`$or`|En az biri sağlanmalı|`OR`|
|`$not`|Değil|`NOT`|
|`$exists`|Alan var mı yok mu|`IS NULL / IS NOT NULL`|
|`$type`|Alan tipi kontrolü|(SQL'de birebir yok)|
|`$regex`|Regex eşleşmesi|`LIKE`, REGEXP|

---

## ✅ Temel Kullanım Örnekleri

### 🔹 1. Eşitlik

```js
db.users.find({ age: 25 })              // age = 25
db.users.find({ age: { $eq: 25 } })     // aynı şey
```

---

### 🔹 2. Karşılaştırma

```js
db.users.find({ age: { $gt: 30 } })         // age > 30
db.users.find({ age: { $gte: 18 } })        // age >= 18
db.users.find({ age: { $lt: 40 } })         // age < 40
db.users.find({ age: { $lte: 65 } })        // age <= 65
```

---

### 🔹 3. Eşit değil

```js
db.users.find({ age: { $ne: 30 } })         // age ≠ 30
```

---

### 🔹 4. `IN` ve `NOT IN`

```js
db.users.find({ city: { $in: ["Ankara", "İzmir"] } })
db.users.find({ city: { $nin: ["Berlin", "Paris"] } })
```

---

### 🔹 5. Birden fazla koşul

```js
db.users.find({
  $and: [
    { age: { $gte: 18 } },
    { city: "İstanbul" }
  ]
})

db.users.find({
  $or: [
    { age: { $lt: 18 } },
    { age: { $gt: 65 } }
  ]
})
```

---

### 🔹 6. Alanın varlığı

```js
db.users.find({ email: { $exists: true } })     // email alanı olanlar
db.users.find({ phone: { $exists: false } })    // phone alanı olmayanlar
```

---

### 🔹 7. Veri tipi kontrolü

```js
db.users.find({ age: { $type: "int" } })
db.users.find({ name: { $type: "string" } })
```

---

### 🔹 8. Regex (LIKE yerine)

```js
db.users.find({ name: { $regex: /^A/, $options: 'i' } })  // A ile başlayan, büyük/küçük harf duyarsız
```

---

### 🔹 9. NOT

```js
db.users.find({ age: { $not: { $gt: 30 } } })  // age <= 30
```

---

### 🔹 10. Çok alanlı koşul (Nokta Notasyonu)

```js
db.orders.find({ "customer.city": "Ankara" })  // İç içe alan
```

---

## 💡 Sonraki Adım

Senin istediğin gibi **100'lerce örnekle çalışmak** için, bu listedeki her operatör için şu sırayla gidebiliriz:

1. Operatör: `$eq` → Sadece bu operatörle 10+ örnek
    
2. Sonra `$ne`, `$gt`, `$gte`, `$lt`, `$lte`…
    
3. Ardından `$in`, `$and`, `$or`, `$exists`, `$regex`, `$type`…
    

🎯 Böylece her operatörü gerçek veri senaryolarıyla öğreneceğiz.

---

İlk olarak hangi operatörle **100'lerce örnek** görmek istersin TkMatE?  
👉 Mesela `$eq`, `$gt` ya da `$and` ile başlayalım mı? Seçtiğini hemen başlatıyorum.