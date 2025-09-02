
`$hour`, bir `Date` değerinden **saat bilgisini** (0 ile 23 arasında) döndürür.

> Saat, UTC zamanına göre değerlendirilir.

---

## 📌 Söz Dizimi

```js
{ $hour: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – Saat Bilgisi Çekme

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      saat: { $hour: "$tarih" }
    }
  }
])
```

### Örnek veri:

```json
{ mesaj: "Giriş yapıldı", tarih: ISODate("2025-07-14T13:45:00Z") }
```

📌 Çıktı:

```json
{ mesaj: "Giriş yapıldı", saat: 13 }
```

> UTC saatine göre 13:45

---

## 🧪 2. `new Date()` ile Şu Anki Saat

```js
db.now.aggregate([
  {
    $project: {
      suAn: new Date(),
      saat: { $hour: new Date() }
    }
  }
])
```

📌 Örnek çıktı:

```json
{ suAn: ISODate("2025-07-14T10:30:00Z"), saat: 10 }
```

---

## 🧪 3. Belirli Saatteki Kayıtları Filtreleme (örneğin saat 9)

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $hour: "$tarih" }, 9 ] }
    }
  }
])
```

> Bu sorgu UTC’ye göre sabah 9 olan kayıtları getirir. (TR saatiyle 12 olabilir)

---

## 🧾 4. Saat Bazlı Gruplama – Hangi Saatte En Yoğun?

```js
db.loglar.aggregate([
  {
    $group: {
      _id: { saat: { $hour: "$tarih" } },
      sayi: { $sum: 1 }
    }
  },
  { $sort: { sayi: -1 } }
])
```

📌 Bu şekilde sistemin en yoğun çalıştığı saatleri bulabilirsin.

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|Saat değeri `0–23` arasıdır|0 = gece 00:00, 23 = 23:59|
|UTC saatine göre çalışır|Türkiye saatine göre 3 saat fark olabilir|
|Tarih değeri `Date` tipinde olmalı|String için `$toDate` gerekebilir|

---

## 🔁 Bonus: Diğer Zaman Operatörleriyle Kombin

```js
db.loglar.aggregate([
  {
    $project: {
      saat: { $hour: "$tarih" },
      dakika: { $minute: "$tarih" },
      saniye: { $second: "$tarih" }
    }
  }
])
```

---

## ✅ Özet

| Operatör                         | Açıklama                           |
| -------------------------------- | ---------------------------------- |
| `$hour`                          | Tarihten saat (0–23) bilgisi döner |
| `$minute`                        | Dakika (0–59)                      |
| `$second`                        | Saniye (0–59)                      |
| UTC tabanlı çalışır              | Türkiye saatine göre fark olabilir |
| Filtreleme ve gruplama destekler | ✅                                  |
