## 1. updateOne()

- **Amaç:** Koşula uyan **ilk** belgeyi günceller.
- **Kullanımı:**

```js
db.collection.updateOne(
   <filter>,         // güncellenecek belgeyi seçmek için koşul
   <update>,         // güncelleme işlemi
   { upsert: <bool> } // (isteğe bağlı) Belge yoksa yeni belge ekler
)
```

- **Örnek:**

```js
db.users.updateOne(
  { username: "ahmet" },
  { $set: { age: 30 } }
)
```

> Bu, username'i "ahmet" olan ilk kullanıcı belgesindeki `age` alanını 30 yapar.

---

## 2. updateMany()

- **Amaç:** Koşula uyan **tüm** belgeleri günceller.
- **Kullanımı:**

```js
db.collection.updateMany(
   <filter>,
   <update>,
   { upsert: <bool> }
)
```

- **Örnek:**

```js
db.users.updateMany(
  { status: "inactive" },
  { $set: { status: "archived" } }
)
```

> `status` alanı "inactive" olan tüm kullanıcıların `status` değerini "archived" yapar.

---

## 3. replaceOne()

- **Amaç:** Koşula uyan **ilk** belgeyi tamamen verilen belgeyle değiştirir (bütün belgeyi).
- **Kullanımı:**

```js
db.collection.replaceOne(
  <filter>,
  <replacementDocument>,
  { upsert: <bool> }
)
```

- **Örnek:**

```js
db.users.replaceOne(
  { username: "ayse" },
  { username: "ayse", age: 25, status: "active" }
)
```

> `username` "ayse" olan belgeyi tamamen verilen belgeyle değiştirir.

---

## 4. findOneAndUpdate()

- **Amaç:** Koşula uyan **ilk** belgeyi günceller ve güncellenmiş belgeyi döner.
- **Kullanımı:**

```js
db.collection.findOneAndUpdate(
  <filter>,
  <update>,
  {
    returnNewDocument: true,  // güncellenmiş belgeyi döner
    upsert: false             // yoksa ekleme yapar mı
  }
)
```

- **Örnek:**
    

```js
db.users.findOneAndUpdate(
  { username: "mehmet" },
  { $inc: { age: 1 } },
  { returnNewDocument: true }
)
```

> `username` "mehmet" olan kullanıcının yaşını 1 artırır ve güncellenmiş belgeyi döner.

---

# Güncelleme Operatörleri

MongoDB update işlemlerinde genellikle şu **operatörler** kullanılır:

|Operatör|Açıklama|Örnek|
|---|---|---|
|`$set`|Alanın değerini değiştirir|`{ $set: { age: 30 } }`|
|`$unset`|Alanı belgede kaldırır|`{ $unset: { age: "" } }`|
|`$inc`|Sayısal alanı artırır/azaltır|`{ $inc: { score: 5 } }`|
|`$push`|Diziye eleman ekler|`{ $push: { tags: "yeni" } }`|
|`$pop`|Diziden ilk veya son elemanı çıkarır|`{ $pop: { tags: 1 } }` (son eleman)|
|`$addToSet`|Diziye tekrarsız eleman ekler|`{ $addToSet: { tags: "unique" } }`|
|`$rename`|Alan adını değiştirir|`{ $rename: { oldName: "newName" } }`|

---

# Örnekler

### 1. `$set` ile güncelleme

```js
db.products.updateOne(
  { _id: 1 },
  { $set: { price: 150 } }
)
```

### 2. `$inc` ile sayısal artış

```js
db.counters.updateOne(
  { _id: "productid" },
  { $inc: { seq: 1 } }
)
```

### 3. Diziye eleman ekleme (`$push`)

```js
db.users.updateOne(
  { username: "ali" },
  { $push: { hobbies: "futbol" } }
)
```

---

# upsert Nedir?

- `upsert: true` seçeneği, update işlemi için koşula uyan belge yoksa **yeni belge ekler**.
- Varsayılan `false`tur.

```js
db.users.updateOne(
  { username: "emre" },
  { $set: { age: 40 } },
  { upsert: true }
)
```

> Eğer `username: "emre"` yoksa yeni belge ekler.

---

# Özet

|Method|Etki Alanı|Yeni Belge Ekleme (Upsert)|Güncellenmiş Belge Döndürme|
|---|---|---|---|
|`updateOne`|İlk belge|Opsiyonel|Hayır|
|`updateMany`|Tüm uygun belgeler|Opsiyonel|Hayır|
|`replaceOne`|İlk belge|Opsiyonel|Hayır|
|`findOneAndUpdate`|İlk belge|Opsiyonel|Evet|

---

İstersen örnek kodlar veya spesifik kullanım senaryoları ile de detaylandırabilirim! MongoDB update konusunda başka merak ettiğin şey var mı?