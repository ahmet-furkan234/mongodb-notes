
`$week`, bir `Date` değerinden **yılın kaçıncı haftasında** olduğunu döner.

- Haftalar 1 ile 53 arasında numaralandırılır.
- Haftanın başlangıcı Pazar veya Pazartesi olabilir (MongoDB sürümüne ve ayarlara bağlı olarak).
- Ancak `$week` Amerikan hafta sistemine göre çalışır (hafta Pazar başlar).

---

## 📌 Söz Dizimi

```js
{ $week: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – Haftanın Numarasını Alma

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      hafta: { $week: "$tarih" }
    }
  }
])
```

### Örnek veri:

```json
{ mesaj: "İşlem tamamlandı", tarih: ISODate("2025-01-10T10:00:00Z") }
```

### Çıktı:

```json
{ mesaj: "İşlem tamamlandı", hafta: 2 }
```

---

## 🧪 2. `new Date()` ile Güncel Haftayı Alma

```js
db.rapor.aggregate([
  {
    $project: {
      bugun: new Date(),
      hafta: { $week: new Date() }
    }
  }
])
```

---

## 🧪 3. Haftaya Göre Filtreleme (örnek: 10. haftadaki kayıtlar)

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $week: "$tarih" }, 10 ] }
    }
  }
])
```

---

## 🧾 4. Haftaya Göre Gruplama

```js
db.loglar.aggregate([
  {
    $group: {
      _id: { hafta: { $week: "$tarih" } },
      toplam: { $sum: 1 }
    }
  },
  { $sort: { "_id.hafta": 1 } }
])
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|Tarih tipi `Date` olmalı|Değilse `$toDate` ile dönüştür|
|Haftanın başlangıcı Pazar (Amerikan formatı)|ISO haftası değil|
|Haftalar 1-53 arasında|Yılın ilk haftası farklı hesaplanabilir|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$week`|Yılın Amerikan hafta numarasını döner|
|Haftalar 1-53 arası|Pazar başlar|
|ISO haftası değil|Avrupa standardından farklı|
|Raporlama ve analizlerde kullanılır|✅|
