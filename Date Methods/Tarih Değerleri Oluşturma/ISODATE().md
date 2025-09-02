
`ISODate()` — MongoDB’de **tarih ve saatleri tutmak için kullanılan özel bir Date (datetime) nesnesidir**.  
`ISO 8601` standart formatında saklanır.

### 🔹 Format:

```js
ISODate("YYYY-MM-DDTHH:MM:SSZ")
```

Örnek:

```js
ISODate("2024-12-31T21:00:00Z")
```

- `T` → tarihi saatten ayırır
- `Z` → UTC zaman dilimini temsil eder (Z = Zulu time = UTC)

---

## 📥 1. ISODate ile Veri Ekleme

### 🔹 Shell üzerinden:

```js
db.loglar.insertOne({
  mesaj: "Sisteme giriş yapıldı",
  tarih: ISODate("2025-07-14T10:00:00Z")
})
```

### 🔹 JavaScript (shell) ile:

```js
new Date("2025-07-14T10:00:00Z")  // Aynı sonucu verir
```

> `ISODate()` sadece shell'de kısa yazım biçimidir. `new Date(...)` ile aynıdır.

---

## 🔎 2. ISODate ile Sorgulama

### 🔹 Belirli tarihteki kayıtları getir:

```js
db.loglar.find({
  tarih: ISODate("2025-07-14T10:00:00Z")
})
```

### 🔹 Tarihten büyük olanları getir:

```js
db.loglar.find({
  tarih: { $gt: ISODate("2025-01-01") }
})
```

### 🔹 Tarih aralığı:

```js
db.loglar.find({
  tarih: {
    $gte: ISODate("2025-01-01"),
    $lte: ISODate("2025-12-31")
  }
})
```

---

## 📆 3. Tarih Otomatik Atama (`Date.now`)

Ekleme sırasında anlık tarih için:

```js
db.loglar.insertOne({
  mesaj: "Anlık log",
  tarih: new Date()  // ISODate olarak otomatik kaydedilir
})
```

---

## 🧠 4. ISODate ile Saat Dönüşümleri (Yerel Zaman vs UTC)

MongoDB tüm tarihleri **UTC (Zaman dilimsiz)** olarak saklar. Ancak sorgularını **yerel saat dilimine göre yazabilirsin**:

```js
new Date("2025-07-14T13:00:00+03:00")  // Türkiye saati
```

➡️ MongoDB bunu UTC’ye dönüştürür:  
`ISODate("2025-07-14T10:00:00Z")`

---

## 🧪 5. ISODate Kullanım Alanları

|Kullanım|Açıklama|
|---|---|
|`insert` işlemlerinde tarih atamak|`ISODate("...")`|
|Tarih aralıkları filtreleme|`$gte`, `$lte`, `$lt`, `$gt`|
|`createdAt`, `updatedAt` alanları oluşturma|`new Date()`|
|Otomatik tarih saklama|`$currentDate` (update için)|
|Pipeline içinde tarih işlemleri|`$year`, `$month`, `$dayOfMonth`, vb. ile|

---

## ✅ Örnek Uygulama

```js
db.etkinlikler.insertMany([
  { ad: "Yaz Kampı", tarih: ISODate("2025-07-01T10:00:00Z") },
  { ad: "Güz Dönemi", tarih: ISODate("2025-10-01T10:00:00Z") }
])

db.etkinlikler.find({
  tarih: { $gte: ISODate("2025-07-01"), $lt: ISODate("2025-09-01") }
})
```

---

## 🧮 Bonus: Tarih İçin `$currentDate` ile Güncelleme

```js
db.loglar.updateOne(
  { _id: ObjectId("...") },
  { $currentDate: { tarih: true } }
)
```

---

## 📌 Özet

|Komut|Açıklama|
|---|---|
|`ISODate("...")`|Shell'de tarih nesnesi oluşturur|
|`new Date()`|Anlık tarih|
|`db.collection.find({ tarih: { $gte: ISODate("...") } })`|Tarihe göre filtreleme|
|`db.collection.updateOne(..., { $currentDate: { tarih: true } })`|Güncel tarih atama|

