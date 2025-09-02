
`$second`, bir `Date` değerinden **saniye** (0-59 arası) bilgisini döner.

---

## 📌 Söz Dizimi

```js
{ $second: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – Saniye Bilgisini Alma

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      saniye: { $second: "$tarih" }
    }
  }
])
```

### Örnek veri:

```json
{ mesaj: "İşlem tamamlandı", tarih: ISODate("2025-07-14T12:30:45Z") }
```

### Çıktı:

```json
{ mesaj: "İşlem tamamlandı", saniye: 45 }
```

---

## 🧪 2. `new Date()` ile Güncel Saniye

```js
db.rapor.aggregate([
  {
    $project: {
      suAn: new Date(),
      saniye: { $second: new Date() }
    }
  }
])
```

---

## 🧪 3. Saniye Bazlı Filtreleme (örnek: saniye 30 olan kayıtlar)

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $second: "$tarih" }, 30 ] }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

- Tarih alanı mutlaka `Date` tipinde olmalı.
    
- Saniye değeri 0-59 arasıdır.
    
- UTC zamanına göre çalışır.
    

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$second`|Tarihten saniye bilgisini döner|
|Değer aralığı|0 ile 59 arasında|
|Kullanım alanı|Zaman analizleri|

---

Sıradaki fonksiyon hangisi olsun?

- `$dateDiff`
    
- `$dateAdd`
    
- `$dateToString`
    
- `$dateTrunc`