
`$dateTrunc`, bir `Date` değerini belirli bir zaman birimine (gün, ay, hafta, yıl, saat vs.) **yuvarlayarak keser**.

> Örneğin: `"2025-07-14T13:45:22"` → Gün seviyesinde truncate → `"2025-07-14T00:00:00"`

---

## 📌 Söz Dizimi

```js
{
  $dateTrunc: {
    date: <tarih>,
    unit: "<birim>",
    binSize: <opsiyonel>,
    timezone: "<opsiyonel>",
    startOfWeek: "<opsiyonel>" // sadece week birimi için
  }
}
```

---

## 🧪 Geçerli `unit` Değerleri

|Unit|Anlamı|
|---|---|
|`"year"`|Yıl|
|`"quarter"`|Çeyrek yıl|
|`"month"`|Ay|
|`"week"`|Hafta|
|`"day"`|Gün|
|`"hour"`|Saat|
|`"minute"`|Dakika|
|`"second"`|Saniye|
|`"millisecond"`|Milisaniye|

---

## 🧪 1. Günlük Truncate – Saati Sıfırlama

```js
db.loglar.aggregate([
  {
    $project: {
      tarih: "$tarih",
      sadeceGun: {
        $dateTrunc: {
          date: "$tarih",
          unit: "day"
        }
      }
    }
  }
])
```

📌 Örnek:  
`"2025-07-14T13:45:22"` → `"2025-07-14T00:00:00Z"`

---

## 🧪 2. Aylık Truncate – Ayın 1’i

```js
{
  $dateTrunc: {
    date: "$tarih",
    unit: "month"
  }
}
```

📌 Örnek:  
`"2025-07-14"` → `"2025-07-01T00:00:00Z"`

---

## 🧪 3. Saatlik Truncate (binSize: 3 saatlik dilim)

```js
{
  $dateTrunc: {
    date: "$tarih",
    unit: "hour",
    binSize: 3
  }
}
```

📌 Örnek:  
`13:40` → `"12:00"`  
`15:10` → `"15:00"`

---

## 🧪 4. Haftaya Yuvarlama (Pazartesi başlasın)

```js
{
  $dateTrunc: {
    date: "$tarih",
    unit: "week",
    startOfWeek: "Monday"
  }
}
```

---

## 🧪 5. Saat Dilimi Kullanımı

```js
{
  $dateTrunc: {
    date: "$tarih",
    unit: "day",
    timezone: "Europe/Istanbul"
  }
}
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`date` zorunludur|`Date` formatında olmalı|
|`unit` çok önemlidir|`"day"`, `"month"`, `"hour"` vb.|
|`binSize` sadece `hour`, `minute`, `second` gibi kısa aralıklar için|Örn: 3 saatlik dilim|
|`startOfWeek` yalnızca `"week"` için geçerli|`"Monday"`, `"Sunday"` gibi|
|Saat dilimi UTC'dir|`timezone` ile değiştirilir|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dateTrunc`|Tarihi belirtilen birime **yuvarlar/keser**|
|Günlük, saatlik, aylık gibi kullanılır|📅|
|`binSize` ile dilimler yapılabilir|3'er saat, 5'er dakika vs.|
|`timezone` ve `startOfWeek` desteklenir|Gelişmiş esneklik sağlar|

---

✅ Artık tüm **date transformation ve date math** operatörlerini büyük ölçüde tamamladık.

Dilersen sıradaki aşamada:

- 🔁 `$switch`, `$cond`, `$ifNull`, `$ifNotNull` gibi **şart (conditional) operatörlerine**
    
- 📦 `$arrayElemAt`, `$slice`, `$map`, `$filter` gibi **array operatörlerine**
    
- 📊 `$group`, `$bucket`, `$sortByCount`, `$facet` gibi **aggregation power tools**’lara geçebiliriz.
    

Ne gelsin kral?