
`$dayOfWeek`, bir `Date` (ISODate) değerinden **haftanın kaçıncı günü** olduğunu döner.

### 🔸 Günler sayısal olarak gelir:

|Gün|Sayı|
|---|---|
|**Pazar**|1|
|Pazartesi|2|
|Salı|3|
|Çarşamba|4|
|Perşembe|5|
|Cuma|6|
|Cumartesi|7|

> ⚠️ **Pazar = 1**, bu MongoDB’nin default `ISO 8601` dışı haftalık formatıdır. (Amerikan sistemi)

---

## 📌 Söz Dizimi

```js
{ $dayOfWeek: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – Gün Numarasını Alma

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      gunNo: { $dayOfWeek: "$tarih" }
    }
  }
])
```

📌 Örnek çıktı:

```json
{ mesaj: "Giriş", gunNo: 2 } // Pazartesi
```

---

## 🧪 2. `new Date()` ile Bugün Haftanın Kaçıncı Günü?

```js
db.sistem.aggregate([
  {
    $project: {
      gunNo: { $dayOfWeek: new Date() }
    }
  }
])
```

📌 Çıktı örneği: `2` → Pazartesi

---

## 🧪 3. Belirli Günlerdeki Kayıtlar (örneğin Cumartesi)

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $dayOfWeek: "$tarih" }, 7 ] }
    }
  }
])
```

➡️ Bu sorgu **Cumartesi** gününe denk gelen kayıtları getirir.

---

## 🧾 4. Haftanın Gününe Göre Gruplama – En Yoğun Gün

```js
db.loglar.aggregate([
  {
    $group: {
      _id: { gun: { $dayOfWeek: "$tarih" } },
      toplam: { $sum: 1 }
    }
  },
  { $sort: { toplam: -1 } }
])
```

📌 Hangi gün kaç kayıt geldiğini görebilirsin.

---

## 🎯 Gün İsmiyle Gösterim İçin Dönüştürme (Opsiyonel)

### Gün numarasını gün ismine dönüştürmek için `$switch` kullanabilirsin:

```js
{
  $switch: {
    branches: [
      { case: { $eq: [ { $dayOfWeek: "$tarih" }, 1 ] }, then: "Pazar" },
      { case: { $eq: [ { $dayOfWeek: "$tarih" }, 2 ] }, then: "Pazartesi" },
      { case: { $eq: [ { $dayOfWeek: "$tarih" }, 3 ] }, then: "Salı" },
      { case: { $eq: [ { $dayOfWeek: "$tarih" }, 4 ] }, then: "Çarşamba" },
      { case: { $eq: [ { $dayOfWeek: "$tarih" }, 5 ] }, then: "Perşembe" },
      { case: { $eq: [ { $dayOfWeek: "$tarih" }, 6 ] }, then: "Cuma" },
      { case: { $eq: [ { $dayOfWeek: "$tarih" }, 7 ] }, then: "Cumartesi" }
    ],
    default: "Bilinmeyen Gün"
  }
}
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|Gün numarası 1'den başlar|`1 = Pazar`, `7 = Cumartesi`|
|Tarih `Date` formatında olmalı|Değilse `toDate` ile dönüştür|
|ISO 8601 standardı kullanılmaz|`Pazartesi = 1` değil! Amerikan formatı!|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dayOfWeek`|Haftanın gün numarasını döner (1 = Pazar, 7 = Cumartesi)|
|`$expr` ile filtreleme yapılabilir|✅|
|Gün ismini göstermek istersen|`$switch` kullanabilirsin|
|Raporlama, analiz, yoğunluk hesaplama|İçin çok kullanışlı|

---

Hazırsan TkMatE, şimdi sıradaki tarih fonksiyonuna geçebiliriz:

- ⏳ `$dateDiff` → 2 tarih arasındaki farkı hesaplar
    
- ➕ `$dateAdd`
    
- ➖ `$dateSubtract`
    
- 🔠 `$dateToString`
    
- 📆 `$dateTrunc`
    

Hangisi gelsin sıradaki yıldız?