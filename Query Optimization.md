
Sorguların:
- Daha **hızlı** çalışması
- Daha **az kaynak tüketmesi**
- Daha **az veri taramasıyla** sonuç döndürmesi

MongoDB’nin **engine**’i sorguları otomatik optimize etmeye çalışır, ama geliştirici olarak en iyi performansı elde etmek için bazı kuralları bilmelisin.

---

## 📦 1. `explain()` ile Sorgu Analizi

```js
db.collection.find({ ... }).explain("executionStats")
```

> Bu komut sorgunun nasıl çalıştığını, index kullanıp kullanmadığını, kaç belge taradığını gösterir.

### 🔍 Dikkat Etmen Gereken Alanlar:

- **`totalDocsExamined`** → Kaç belge tarandı?
- **`totalKeysExamined`** → Kaç index girdisi kullanıldı?
- **`executionTimeMillis`** → Sorgu süresi
- **`stage`** → COLLSCAN (tam tarama ❌) vs IXSCAN (index taraması ✅)

---

## 🧩 2. Sorguda İndex Kullanımına Uygun Yazım

### ❌ Kötü:

```js
db.users.find({ $or: [ { name: "Ali" }, { age: 30 } ] })
```

> Bu sorgu, iki alan için ayrı ayrı index taraması yerine tam tarama yapabilir.

### ✅ İyi:

```js
db.users.createIndex({ name: 1, age: 1 })

db.users.find({ name: "Ali", age: 30 })
```

> Compound index kullanımı doğrudan performansı artırır.

---

## 🔍 3. Sorgu Yapısı Optimizasyonu

|Kural|Açıklama|
|---|---|
|`select *` gibi tüm alanları getirme|Sadece ihtiyacın olan alanları al (`{ field1: 1, field2: 1 }`)|
|`$regex` → yavaş olabilir|Mümkünse kaçın veya anchored pattern kullan|
|`$where` → çok yavaş|JavaScript engine kullanır, production’da önerilmez|
|`$in`, `$nin`, `$ne` → dikkatli kullan|Büyük listelerde yavaşlayabilir|

---

## ⚙️ 4. `covered query` kullanımı

> Hem sorgu hem de proje edilen alanlar index üzerinden karşılanıyorsa, MongoDB belgeleri hiç okumadan sonucu döndürür (çok hızlıdır).

```js
db.products.createIndex({ category: 1, price: 1 })

db.products.find(
  { category: "electronics", price: { $lt: 500 } },
  { _id: 0, category: 1, price: 1 } // sadece indexteki alanlar!
)
```

> Bu bir **covered query**’dir ✅

---

## 🧱 5. Doğru Index Kullanımı

### 📌 Tek Alan Index

```js
db.users.createIndex({ name: 1 })
```

### 📌 Compound Index (çoklu alan)

```js
db.users.createIndex({ name: 1, age: -1 })
```

> Önemli: Compound index’lerde sıralama ve sıralı sorgular dikkat ister!

### 📌 Sıralama ile uyum

```js
db.users.find({ name: "Ahmet" }).sort({ age: -1 }) // age ikinci alan, uyumlu
```

---

## 📍 6. `$lookup` ve `$group` Optimizasyonu

### 🔹 `$lookup` sonrası `$match` ekle:

```js
{
  $lookup: {
    from: "orders",
    localField: "userId",
    foreignField: "_id",
    as: "orders"
  }
},
{ $match: { "orders.status": "paid" } }
```

> Join sonrası filtreyi hemen uygula, fazla veriyi taşımamış olursun.

### 🔹 `$group` öncesi `$match` kullan:

```js
{ $match: { status: "active" } },
{ $group: { _id: "$country", count: { $sum: 1 } } }
```

> Daha az veri = daha hızlı `$group`

---

## 📊 7. `hint()` ile Index Zorlaması (İleri Seviye)

```js
db.orders.find({ status: "shipped" }).hint({ status: 1 })
```

> Belirli index kullanılması **zorunlu kılınabilir**, ancak dikkatli kullanılmalıdır.

---

## 🔥 Gerçek Dünya Önerileri

|Durum|Öneri|
|---|---|
|Sık filtrelenen alanlar|İndeksle destekle|
|Sıralama kullanıyorsan|`sort()` ile uyumlu index oluştur|
|Fazla alan getiriyorsan|Yalnızca gereken alanları proje et (`projection`)|
|Çok büyük koleksiyonlarda|`limit`, `skip` yerine sayfalama (`range based`) önerilir|
|Ağır sorgular|`explain()` ve `profiler` ile analiz et|

---

## 🧠 Bonus: Query Profiler

```js
db.setProfilingLevel(2) // Tüm yavaş sorguları logla
db.system.profile.find().sort({ millis: -1 }).limit(5)
```

> En yavaş sorguları gör ve düzeltmeye buradan başla.

---

## ✅ Özet

|Optimizasyon Noktası|Açıklama|
|---|---|
|`explain()`|Sorgunun nasıl çalıştığını gör|
|Index uyumlu sorgu yazımı|Performansı doğrudan etkiler|
|Az veri taşı, az alan getir|Hafif sorgular oluştur|
|`$match` ve `$project` öncelikli|Pipeline başında kullan|
|Yavaş operatörlerden kaçın|`$where`, `$regex`, `$ne`, `$nin` dikkatli kullanılmalı|
