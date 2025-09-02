
`$minute`, bir `Date` değerinden **dakika** (0-59 arası) bilgisini döner.

---

## 📌 Söz Dizimi

```js
{ $minute: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – Dakika Bilgisini Alma

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      dakika: { $minute: "$tarih" }
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
{ mesaj: "İşlem tamamlandı", dakika: 30 }
```

---

## 🧪 2. `new Date()` ile Güncel Dakika

```js
db.rapor.aggregate([
  {
    $project: {
      suAn: new Date(),
      dakika: { $minute: new Date() }
    }
  }
])
```

---

## 🧪 3. Dakika Bazlı Filtreleme (örnek: dakika 15 olan kayıtlar)

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $minute: "$tarih" }, 15 ] }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

- Tarih alanı mutlaka `Date` tipinde olmalı.
    
- Dakika değeri 0-59 arasıdır.
    
- UTC zamanına göre çalışır.
    

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$minute`|Tarihten dakika bilgisini döner|
|Değer aralığı|0 ile 59 arasında|
|Kullanım alanı|Zaman analizleri|

---

Sıradaki fonksiyon hangisi olsun?

- `$second`
    
- `$dateDiff`
    
- `$dateAdd`
    
- `$dateToString`
    
- `$dateTrunc`