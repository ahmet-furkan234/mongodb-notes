
## 🧠 Index (İndeks) Nedir?

- İndeksler, veritabanındaki **belirli alanlara göre veriye daha hızlı erişmek** için kullanılır.
- **SQL'deki INDEX yapısının** MongoDB karşılığıdır.
- Sorgular indeks kullanırsa çok daha hızlı çalışır.

---

## 🛠️ Temel Komut: `createIndex()`

```js
db.collection.createIndex({ alan: 1 })
```

- `1`: Artan sıralama (ascending)
- `-1`: Azalan sıralama (descending)

> 🔍 Not: Sıralama sadece **çoklu index** durumlarında önemlidir.

---

## ✅ 1. Tek Alanlı (Single Field) Index

```js
db.users.createIndex({ email: 1 })
```

➡️ `email` alanına göre aramalar çok daha hızlı hale gelir:

```js
db.users.find({ email: "tkmate@example.com" })
```

---

## ✅ 2. Çok Alanlı (Compound) Index

```js
db.orders.createIndex({ userId: 1, createdAt: -1 })
```

➡️ Bu, şu sorguları optimize eder:

```js
db.orders.find({ userId: 123 })                      // OK ✅
db.orders.find({ userId: 123 }).sort({ createdAt: -1 }) // OK ✅
db.orders.find({ createdAt: -1 })                    // ❌ İndeks kullanılmaz
```

> 💡 Çok alanlı indexlerde **başlangıçtaki alan** her zaman aranmalıdır!

---

## ✅ 3. Unique Index (Benzersiz)

```js
db.users.createIndex({ username: 1 }, { unique: true })
```

➡️ Aynı kullanıcı adını ikinci kez eklemeye çalışırsan hata alırsın.

---

## ✅ 4. TTL Index (Zamanlayıcı İndeks – Otomatik Silinen Belgeler)

```js
db.logs.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })
```

➡️ Belge `createdAt` tarihinden 1 saat sonra **otomatik silinir**. Çok güzel bir log temizleyicidir.

---

## ✅ 5. Sparse Index (Eksik Alanlar için)

```js
db.products.createIndex({ discount: 1 }, { sparse: true })
```

➡️ `discount` alanı olmayan belgeler **indexe dahil edilmez**.

---

## ✅ 6. Partial Index (Koşullu İndeks)

```js
db.orders.createIndex(
  { status: 1 },
  { partialFilterExpression: { status: { $exists: true } } }
)
```

➡️ Sadece `status` alanı olan belgeler için indeks oluşturur.

---

## ✅ 7. Text Index (Metinsel Arama)

```js
db.articles.createIndex({ title: "text", content: "text" })
```

```js
db.articles.find({ $text: { $search: "mongodb index" } })
```

---

## ✅ 8. Wildcard Index (Dinamik alanlar için)

```js
db.settings.createIndex({ "preferences.$**": 1 })
```

➡️ JSON içinde bilinmeyen dinamik key'ler varsa onları kapsar.

---

## 🎯 Ekstra Komutlar

### 📋 Tüm indexleri listele:

```js
db.collection.getIndexes()
```

### ❌ Index sil:

```js
db.collection.dropIndex("index_adi")
```

### 🧼 Tüm indexleri sil:

```js
db.collection.dropIndexes()
```

---

## 🧪 Performans Testi: İndeks Kullanıldı mı?

```js
db.collection.find({ alan: "değer" }).explain("executionStats")
```

İçinde `"IXSCAN"` varsa = ✅ index kullanılmış.

---

## 🧠 Stratejik Öneriler

|Durum|Kullanılacak Index Türü|
|---|---|
|Arama yapılacak alan|Single / Compound Index|
|Çok veri varsa|Compound + Sort ile|
|Eşsiz veri gerekliyse|Unique Index|
|Zamanla silinmesi gerekenler|TTL Index|
|Aranacak metin varsa|Text Index|
|Eksik veri varsa|Sparse / Partial Index|
