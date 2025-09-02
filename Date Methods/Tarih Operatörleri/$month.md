
**`$month`**, bir tarih değerinden **ayın numarasını (1–12)** döner.

### 📌 Kullanım Formatı:

```js
{ $month: <tarih ifadesi> }
```

---

## 🧪 1. Basit Kullanım – Ay Bilgisi Çekme

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      ay: { $month: "$tarih" }
    }
  }
])
```

### Örnek veri:

```json
{ mesaj: "Giriş yapıldı", tarih: ISODate("2025-07-14T12:30:00Z") }
```

### Çıktı:

```json
{ mesaj: "Giriş yapıldı", ay: 7 }
```

---

## 🧪 2. Anlık Ay: `new Date()` ile Kullanım

```js
db.rapor.aggregate([
  {
    $project: {
      bugunkuAy: { $month: new Date() }
    }
  }
])
```

➡️ Çıktı örneği (Temmuz için): `7`

---

## 🧪 3. Belirli Ay’a Göre Filtreleme (örneğin Temmuz ayındaki kayıtlar)

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $month: "$tarih" }, 7 ] }
    }
  }
])
```

---

## 🧾 4. Ay Bazlı Gruplama – Aylık Raporlama

```js
db.siparisler.aggregate([
  {
    $group: {
      _id: { ay: { $month: "$siparisTarihi" } },
      siparisSayisi: { $sum: 1 }
    }
  },
  {
    $sort: { "_id.ay": 1 }
  }
])
```

### Örnek çıktı:

```json
{ "_id": { "ay": 1 }, "siparisSayisi": 231 }
{ "_id": { "ay": 2 }, "siparisSayisi": 198 }
// ...
{ "_id": { "ay": 12 }, "siparisSayisi": 347 }
```

---

## ⚠️ Dikkat: Tarih Tipi Doğru Olmalı

Eğer `"tarih"` alanın `Date` türünde değilse (örneğin `"2025-07-14"` şeklinde bir string ise), önce `toDate` dönüşümü yapmalısın:

```js
{ $month: { $toDate: "$tarihString" } }
```

---

## 🧠 Bonus: Yıl–Ay Gruplaması (detaylı rapor)

```js
db.siparisler.aggregate([
  {
    $group: {
      _id: {
        yil: { $year: "$siparisTarihi" },
        ay: { $month: "$siparisTarihi" }
      },
      toplam: { $sum: "$tutar" }
    }
  },
  { $sort: { "_id.yil": 1, "_id.ay": 1 } }
])
```

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$month`|Tarihten ay numarası döner (1–12)|
|`match` içinde kullanımı|`$expr` ile yapılır|
|Ay bazlı raporlar|`$group` ile|
|`string` tarih varsa|`$toDate` ile dönüştür|
|UTC tabanlı çalışır|Türkiye saatine göre kaymalara dikkat|

---

Sıradaki adımda **`$dayOfMonth`**, **`$dayOfWeek`**, **`$dateDiff`**, **`$dateAdd`**, ya da **`$dateToString`** gibi tarih operatörlerinden birine geçebiliriz.

Hangisinden devam edelim TkMatE?