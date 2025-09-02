
`$dayOfMonth`, bir `Date` (ISODate) alanından **ayın kaçıncı günü** olduğunu döner.

---

## 📌 Söz Dizimi

```js
{ $dayOfMonth: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – Gün Bilgisi Çekme

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      gun: { $dayOfMonth: "$tarih" }
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
{ mesaj: "Giriş yapıldı", gun: 14 }
```

---

## 🧪 2. `new Date()` ile Kullanım – Bugünün Günü

```js
db.sistem.aggregate([
  {
    $project: {
      bugun: { $dayOfMonth: new Date() }
    }
  }
])
```

📌 Çıktı örneği: `14`

---

## 🧪 3. Gün Bazlı Filtreleme

### Örnek: Ayın 1’i olan kayıtları getir

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $dayOfMonth: "$tarih" }, 1 ] }
    }
  }
])
```

---

## 🧾 4. Gün Bazlı Gruplama – En çok hangi günde işlem olmuş?

```js
db.loglar.aggregate([
  {
    $group: {
      _id: { gun: { $dayOfMonth: "$tarih" } },
      sayi: { $sum: 1 }
    }
  },
  { $sort: { sayi: -1 } }
])
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|Tarih alanı `Date` tipinde olmalı|Aksi halde hata alırsın|
|`string` tipinde tarih varsa|`toDate` ile dönüştür|

```js
{ $dayOfMonth: { $toDate: "$tarihString" } }
```

---

## 🔁 Kombinasyon: Yıl, Ay, Gün

```js
db.loglar.aggregate([
  {
    $project: {
      yil: { $year: "$tarih" },
      ay: { $month: "$tarih" },
      gun: { $dayOfMonth: "$tarih" }
    }
  }
])
```

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dayOfMonth`|Tarihten gün (1–31) değerini döner|
|`match` ile kullanımı|`$expr` gerektirir|
|Ay başı, ay ortası analizleri|Kolaylaştırır|
|Tarih tipinde dikkat|`Date` değilse dönüştür (`$toDate`)|

---

Hazırsan sıradaki fonksiyonlardan birine geçebiliriz:

- 🔁 `$dayOfWeek`
    
- 🧮 `$dateDiff`
    
- ➕ `$dateAdd`
    
- 🔁 `$dateToString`
    
- 🧭 `$dateSubtract`
    
- 📆 `$dateTrunc`
    

Hangisini seçelim TkMatE?