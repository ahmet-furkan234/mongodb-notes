
**`$dayOfYear`**, bir `Date` değerinden **yılın kaçıncı günü** olduğunu döner.

> 1 Ocak = 1  
> 31 Aralık = 365 veya 366 (artık yıla göre)

---

## 📌 Söz Dizimi

```js
{ $dayOfYear: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – Gün Sırası Çekme

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      yilGunu: { $dayOfYear: "$tarih" }
    }
  }
])
```

### Örnek veri:

```json
{ mesaj: "İşlem yapıldı", tarih: ISODate("2025-01-01T10:00:00Z") }
```

📌 Çıktı:

```json
{ mesaj: "İşlem yapıldı", yilGunu: 1 }
```

---

## 🧪 2. `new Date()` ile Bugün Yılın Kaçıncı Günü?

```js
db.rapor.aggregate([
  {
    $project: {
      bugun: new Date(),
      gunSirasi: { $dayOfYear: new Date() }
    }
  }
])
```

📌 Örnek çıktı (örneğin 14 Temmuz):

```json
{ bugun: ISODate("2025-07-14T..."), gunSirasi: 195 }
```

---

## 🧪 3. Belirli Gün Numarasına Göre Filtreleme

### Örnek: Yılın 200. günü olan kayıtlar:

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $dayOfYear: "$tarih" }, 200 ] }
    }
  }
])
```

---

## 🧾 4. Yılın Hangi Gününde En Çok Olay Olmuş?

```js
db.loglar.aggregate([
  {
    $group: {
      _id: { gunSirasi: { $dayOfYear: "$tarih" } },
      sayi: { $sum: 1 }
    }
  },
  { $sort: { sayi: -1 } }
])
```

📌 En yoğun günleri görebilirsin.

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`Date` tipi zorunlu|Eğer tarih string ise `toDate` kullan: `{ $dayOfYear: { $toDate: "$tarih" } }`|
|Artık yıllarda maksimum 366|Normalde 365|
|UTC'ye göre hesaplanır|Zaman dilimi farkı olabilir|

---

## 🧠 Bonus: `$dayOfMonth` vs `$dayOfYear` vs `$dayOfWeek`

|Operatör|Açıklama|Değer Aralığı|
|---|---|---|
|`$dayOfMonth`|Ayın günü|1–31|
|`$dayOfYear`|Yılın günü|1–365/366|
|`$dayOfWeek`|Haftanın günü|1–7 (Pazar = 1)|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dayOfYear`|Tarihin yıl içindeki sıra numarasını verir|
|Örnek: 1 Ocak → 1|✅|
|Artık yıllarda 366 olabilir|✅|
|Filtreleme ve gruplama yapılabilir|✅|
|Tarih `Date` tipi olmalı|String için `$toDate` gerekir|

---

Hazırsan TkMatE, bir sonraki tarih operatörüyle devam edebiliriz:

- ⏳ `$dateDiff` – iki tarih arasındaki fark
    
- ➕ `$dateAdd` – tarih üstüne gün, ay vs. ekleme
    
- ➖ `$dateSubtract` – tarihten çıkarma
    
- 🔠 `$dateToString` – tarihi stringe çevirme
    
- 📆 `$dateTrunc` – tarihi yuvarlama (ay başı, hafta başı vs.)
    

Hangisini istersin sırada?