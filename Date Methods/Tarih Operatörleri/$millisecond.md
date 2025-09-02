
`$millisecond`, bir `Date` değerinden **milisaniye** (0-999 arası) değerini döner.  
Yani, saniyenin hangi milisaniyesinde olduğunu verir.

---

## 📌 Söz Dizimi

```js
{ $millisecond: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – Milisaniye Bilgisini Alma

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      milisaniye: { $millisecond: "$tarih" }
    }
  }
])
```

### Örnek veri:

```json
{ mesaj: "İşlem tamamlandı", tarih: ISODate("2025-07-14T12:30:15.123Z") }
```

### Çıktı:

```json
{ mesaj: "İşlem tamamlandı", milisaniye: 123 }
```

---

## 🧪 2. `new Date()` ile Güncel Milisaniye

```js
db.rapor.aggregate([
  {
    $project: {
      suAn: new Date(),
      milisaniye: { $millisecond: new Date() }
    }
  }
])
```

---

## 🧪 3. Milisaniye Bazlı Filtreleme (örnek: milisaniyesi 500 olan kayıtlar)

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $millisecond: "$tarih" }, 500 ] }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

- Tarih tipi mutlaka `Date` olmalı.
    
- Milisaniye 0 ile 999 arasıdır.
    
- Çok nadiren milisaniyeye göre filtreleme veya grup yapılır.
    

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$millisecond`|Tarihin milisaniye kısmını verir|
|Değer aralığı|0 ile 999 arasında|
|Kullanım alanı|Hassas zaman ölçümleri|

---

Sıradaki fonksiyon ne olsun TkMatE?

- ⏳ `$dateDiff`
    
- ➕ `$dateAdd`
    
- ➖ `$dateSubtract`
    
- 🔠 `$dateToString`
    
- 📆 `$dateTrunc`
    

Hangisi?