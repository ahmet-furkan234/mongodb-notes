
`$dateFromString`, bir **string tarih** ifadesini alır ve onu `ISODate` formatına dönüştürür.  
Tarih formatı belirtebilir, saat dilimi ayarlayabilir, hatalı değerler için varsayılan belirleyebilirsin.

---

## 📌 Söz Dizimi

```js
{
  $dateFromString: {
    dateString: <tarih string>,
    format: "<tarih formatı>",        // opsiyonel
    timezone: "<saat dilimi>",        // opsiyonel
    onError: <hatalıysa bu tarihi kullan>,     // opsiyonel
    onNull: <null ise bu tarihi kullan>        // opsiyonel
  }
}
```

---

## 🧪 1. Temel Kullanım – Basit ISO Tarih Dönüşümü

```js
db.veriler.aggregate([
  {
    $project: {
      tarih: {
        $dateFromString: {
          dateString: "2025-07-14T12:30:00Z"
        }
      }
    }
  }
])
```

📌 Çıktı:

```json
{ "tarih": ISODate("2025-07-14T12:30:00Z") }
```

---

## 🧪 2. Özel Formatlı Tarihi Çevirme

```js
db.veriler.aggregate([
  {
    $project: {
      tarih: {
        $dateFromString: {
          dateString: "14-07-2025 15:45",
          format: "%d-%m-%Y %H:%M"
        }
      }
    }
  }
])
```

📌 Çıktı:

```json
{ "tarih": ISODate("2025-07-14T15:45:00Z") }
```

---

## 🧪 3. Saat Dilimi Belirtme

```js
{
  $dateFromString: {
    dateString: "2025-07-14 15:00",
    format: "%Y-%m-%d %H:%M",
    timezone: "Europe/Istanbul"
  }
}
```

---

## 🧪 4. Hatalı Veri İçin Varsayılan Ayarlama

```js
{
  $dateFromString: {
    dateString: "$tarihStr",
    format: "%d.%m.%Y",
    onError: ISODate("2000-01-01T00:00:00Z"),
    onNull: ISODate("1990-01-01T00:00:00Z")
  }
}
```

---

## 🗓️ Yaygın Format Kodları

|Kod|Açıklama|
|---|---|
|`%Y`|4 haneli yıl (2025)|
|`%m`|Ay (01–12)|
|`%d`|Gün (01–31)|
|`%H`|Saat (00–23)|
|`%M`|Dakika (00–59)|
|`%S`|Saniye (00–59)|
|`%L`|Milisaniye|

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`dateString` zorunludur|string olmalı|
|`format` verilmezse ISO 8601 varsayılır|`"2025-07-14T12:00:00Z"` gibi|
|`timezone` UTC varsayılan|`"Europe/Istanbul"` gibi belirtilebilir|
|Hatalı/parçalanamayan tarih|`onError` ile kontrol altına alınmalı|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dateFromString`|String ifadeyi tarihe çevirir|
|`format` desteği var|%Y, %d, %m vs. ile özelleştirebilirsin|
|`timezone`, `onError`, `onNull` parametreleriyle güçlü|✅|
|Dış veri kaynaklarında sık kullanılır|JSON, CSV, Excel tarihleri için ideal|

---

Sırada ne var TkMatE? Şunlarla devam edebiliriz:

- 🔠 `$dateToString` → Tarihi yazıya çevirme
    
- 📆 `$dateTrunc` → Tarihi haftaya, aya, yıla yuvarlama
    
- ➖ `$dateSubtract` → Tarihten zaman çıkarma
    

Hangisini getireyim?