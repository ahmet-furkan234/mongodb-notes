
Bir koleksiyon üzerinde **çok sayıda işlem** (insert, update, delete vb.) **tek bir istekte (batch)** yapılır. Bu, hem performans açısından hızlıdır, hem de veritabanı ile daha az bağlantı kurulmasını sağlar.

---

## ✅ Genel Kullanım Şekli:

```js
db.collection.bulkWrite([
  { insertOne: { document: { ... } } },
  { updateOne: { filter: { ... }, update: { $set: { ... } }, upsert: true } },
  { deleteOne: { filter: { ... } } },
  { replaceOne: { filter: { ... }, replacement: { ... }, upsert: false } }
])
```

---

## 🎯 Desteklenen İşlem Türleri

|İşlem Türü|Açıklama|
|---|---|
|`insertOne`|Yeni belge ekler|
|`updateOne`|İlk eşleşeni günceller|
|`updateMany`|Tüm eşleşenleri günceller|
|`deleteOne`|İlk eşleşeni siler|
|`deleteMany`|Tüm eşleşenleri siler|
|`replaceOne`|İlk eşleşeni komple değiştirir|

---

## 💡 Örnek 1: Karışık bir `bulkWrite` işlemi

```js
db.users.bulkWrite([
  { insertOne: {
      document: { username: "ahmet", age: 25, status: "active" }
  }},
  { updateOne: {
      filter: { username: "ayse" },
      update: { $set: { age: 30 } },
      upsert: true
  }},
  { deleteOne: {
      filter: { username: "mehmet" }
  }},
  { replaceOne: {
      filter: { username: "veli" },
      replacement: { username: "veli", age: 40, city: "Ankara" },
      upsert: false
  }}
])
```

### 🔍 Bu işlemler sırayla şunları yapar:

1. Yeni bir kullanıcı ekler (`insertOne`).
2. `username: "ayse"` olanın yaşını 30 yapar (yoksa `upsert: true` ile ekler).
3. `username: "mehmet"` olan kullanıcıyı siler.
4. `username: "veli"` olan kullanıcıyı tamamen yenisiyle değiştirir.

---

## 📋 Sonuç Yapısı

```json
{
  "acknowledged" : true,
  "insertedCount" : 1,
  "matchedCount" : 2,
  "modifiedCount" : 1,
  "deletedCount" : 1,
  "upsertedCount" : 1,
  "upsertedIds" : {
    "1" : ObjectId("...")
  }
}
```

---

## 🔁 Örnek 2: 100 kullanıcıyı topluca güncelleme

```js
const updates = [];

for (let i = 0; i < 100; i++) {
  updates.push({
    updateOne: {
      filter: { userId: i },
      update: { $inc: { score: 10 } },
      upsert: true
    }
  });
}

db.users.bulkWrite(updates)
```

> Bu kod, `userId` değeri 0'dan 99'a kadar olan her belgeyi bulur, `score` alanını 10 artırır. Yoksa belgeyi `upsert` ile ekler.

---

## 💣 Hata Yönetimi

- Eğer işlemlerden biri başarısız olursa, diğerleri devam eder.
- Dilersen `ordered: false` parametresi ile sıraya bağlı kalmadan çalıştırabilirsin.

```js
db.collection.bulkWrite([...], { ordered: false })
```

> **ordered: true (varsayılan):** İlk hata aldığında durur.  
> **ordered: false:** Hataları atlar, devam eder.

---

## 🧠 Ne Zaman Kullanılır?

- Çok sayıda belgeye aynı anda işlem yapılacaksa (örnek: puan güncelleme, temizlik işlemleri).
- Performans kritikse (az sayıda bağlantıyla çok işlem yapılmalıysa).
- Toplu veri güncellemeleri, arşivleme, senkronizasyon gibi durumlarda.

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|Sıra (ordered)|İşlemler sırayla yapılır (varsayılan), istenirse paralel|
|Limit|Her `bulkWrite` çağrısı 1000 adede kadar işlem içerebilir|
|Operatörler|`update` işlemleri mutlaka `$set`, `$inc`, vs. içermeli|
|`replaceOne`|Operatör kullanılmaz, direkt yeni nesne verilir|
