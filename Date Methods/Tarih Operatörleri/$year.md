
`$year`, bir `Date` (ISODate) alanından **yıl bilgisini (YYYY)** çıkartmak için kullanılır.

---

## 📌 Kullanım Formatı

```js
{ $year: <tarih ifadesi> }
```

🔹 `tarih ifadesi` = genelde bir tarih alanı (`$tarih`) veya `new Date(...)`

---

## 🧪 1. Temel Kullanım – Yıl Bilgisi Çekme

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      yil: { $year: "$tarih" }
    }
  }
])
```

### 📌 Örnek veri:

```json
{ mesaj: "Sisteme giriş", tarih: ISODate("2024-12-31T23:59:59Z") }
```

### 📌 Çıktı:

```json
{ mesaj: "Sisteme giriş", yil: 2024 }
```

---

## 🧪 2. `new Date()` ile Kullanım

```js
db.test.aggregate([
  {
    $project: {
      suAnYili: { $year: new Date() }
    }
  }
])
```

➡️ Bu şekilde anlık yıl değerini alırsın. Örn: `2025`

---

## 🧪 3. Filtreleme İçin Kullanım (match içinde kullanmak için `$expr` gerekir)

### 🎯 Örnek: 2023 yılında yapılmış kayıtlar

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $year: "$tarih" }, 2023 ] }
    }
  }
])
```

---

## 🧠 `$year` Diğer Tarih Operatörleriyle Kombin

|Operatör|Açıklama|Örnek|
|---|---|---|
|`$month`|Ay numarası (1–12)|`{ $month: "$tarih" }`|
|`$dayOfMonth`|Gün (1–31)|`{ $dayOfMonth: "$tarih" }`|
|`$dayOfWeek`|Gün (1=Pazar, 7=Cumartesi)|`{ $dayOfWeek: "$tarih" }`|
|`$hour`, `$minute`, `$second`|Saat bileşenleri|`{ $hour: "$tarih" }`|

---

## 🧾 4. Gerçekçi Örnek: Sipariş Sayısını Yıla Göre Gruplama

```js
db.siparisler.aggregate([
  {
    $group: {
      _id: { yil: { $year: "$siparisTarihi" } },
      toplamSiparis: { $sum: 1 }
    }
  }
])
```

📌 Çıktı örneği:

```json
{ "_id": { "yil": 2023 }, "toplamSiparis": 1482 }
{ "_id": { "yil": 2024 }, "toplamSiparis": 1573 }
```

---

## ⚠️ Dikkat Et: Tarih Formatı Uyumlu Olmalı

Eğer `$year` kullandığın değer `Date` tipinde değilse hata verir.  
Yani string formatlı tarihleri önce dönüştürmen gerekebilir:

```js
{ $year: { $toDate: "$tarihString" } }
```

---

## 🧠 Bonus: Yıl, Ay, Gün Ayrıştırma Aynı Anda

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
|`$year`|Tarihten yıl değerini alır|
|`match` içinde kullanım|`$expr` ile yapılır|
|Gruplama ve raporlamada sık kullanılır|✅|
|Hatalardan kaçınmak için|Tarih formatı `Date` olmalı|
