
`new Date()` JavaScript'in bir fonksiyonudur ve MongoDB shell’de de çalışır çünkü MongoDB shell (mongosh) JavaScript tabanlıdır.

### 🔸 Görevleri:

- Anlık zamanı oluşturur (şu anki tarih ve saat)
- ISO 8601 formatına çevrilebilir
- `ISODate()` ile aynı sonucu verir

---

## 🧪 1. En Basit Kullanım

```js
new Date()
```

📌 Çıktı örneği:

```
ISODate("2025-07-14T12:25:00.000Z")
```

> Not: UTC saatine göre oluşturulur.

---

## 🧪 2. Belirli Bir Tarih Oluşturma

### 🔸 ISO string ile:

```js
new Date("2025-07-01T10:00:00Z")
```

### 🔸 Tarih-saat + zaman dilimi:

```js
new Date("2025-07-14T15:00:00+03:00")  // Türkiye saati
```

➡️ MongoDB bunu otomatik UTC'ye çevirir.

---

### 🔸 Yıl-ay-gün formatıyla (dikkat! ay = 0 tabanlı):

```js
new Date(2025, 6, 14)  // 14 Temmuz 2025 (çünkü ay: 6 = Temmuz)
```

```js
new Date(2025, 6, 14, 10, 30)  // 14 Temmuz 2025 10:30
```

---

## 🧾 3. `new Date()` ile Veri Ekleme

```js
db.loglar.insertOne({
  mesaj: "Kayıt oluşturuldu",
  tarih: new Date()
})
```

---

## 🔍 4. `new Date()` ile Sorgu

```js
db.loglar.find({
  tarih: { $gte: new Date("2025-01-01T00:00:00Z") }
})
```

---

## 🛠️ 5. `new Date()` ile Otomatik Alan Oluşturma

### 🔸 Oluşturulma tarihi (`createdAt`)

```js
db.kullanicilar.insertOne({
  ad: "Ayşe",
  createdAt: new Date()
})
```

---

## 🧮 6. `new Date()` Güncelleme İçin

```js
db.kullanicilar.updateOne(
  { ad: "Ayşe" },
  { $set: { lastLogin: new Date() } }
)
```

---

## ⏳ 7. Anlık Tarih Saklama Alternatifi: `$currentDate`

```js
db.kullanicilar.updateOne(
  { ad: "Ayşe" },
  { $currentDate: { lastLogin: true } }
)
```

Bu da `new Date()` ile aynı işi yapar ama sadece güncellemede kullanılır.

---

## 🧠 `new Date()` vs `ISODate()`

|Özellik|`new Date()`|`ISODate()`|
|---|---|---|
|JavaScript fonksiyonu|✅|❌|
|Mongo shell özel kısayolu|❌|✅|
|Uygulamalarda kullanılır (Node.js, C# vb.)|✅|❌|
|Aynı nesne döner|✅|✅|
|UTC tabanlı tarih üretir|✅|✅|

---

## 🔁 Bonus: `new Date().toISOString()`

İçindeki veriyi string'e çevirmek için:

```js
new Date().toISOString()
// Örnek çıktı: "2025-07-14T12:32:45.000Z"
```

---

## 📌 Özet

|Kullanım|Açıklama|
|---|---|
|`new Date()`|Anlık zaman üretir|
|`new Date("2025-07-01T10:00:00Z")`|Belirli tarih oluşturur|
|`new Date(2025, 6, 14)`|Ay sıfırdan başlar!|
|`db.collection.insert({ tarih: new Date() })`|Güncel tarihi ekler|
|`db.collection.update(..., { $set: { tarih: new Date() } })`|Tarihi günceller|
|Alternatif|`$currentDate` operatörü|

---

Hazırsan şimdi bir sonraki adımda `new Date()` ile birlikte kullanılan **tarih fonksiyonları**na geçebiliriz:

- `$year`, `$month`, `$dayOfWeek`, `$dateDiff`, `$dateTrunc`, `$dateAdd`, `$dateSubtract` gibi...
    

Hangisinden başlayalım TkMatE?