
`$dateFromParts`, tek tek yıl, ay, gün, saat gibi bileşenleri kullanarak **yeni bir `Date` nesnesi oluşturmanı** sağlar.  
Yani, yıl/ay/gün/saat gibi parçaları birleştirip tarih oluşturur.

---

## 📌 Söz Dizimi (UTC varsayılan)

```js
{
  $dateFromParts: {
    year: <yıl>,
    month: <ay>,
    day: <gün>,
    hour: <saat>,
    minute: <dakika>,
    second: <saniye>,
    millisecond: <milisaniye>,
    timezone: "<isteğe bağlı>"
  }
}
```

> Sadece `year`, `month`, `day` zorunludur; diğerleri isteğe bağlıdır.

---

## 🧪 1. Basit Tarih Oluşturma (Yıl, Ay, Gün)

```js
db.test.aggregate([
  {
    $project: {
      tarih: {
        $dateFromParts: {
          year: 2025,
          month: 7,
          day: 14
        }
      }
    }
  }
])
```

📌 Çıktı:

```json
{ "tarih": ISODate("2025-07-14T00:00:00Z") }
```

---

## 🧪 2. Saat, Dakika, Saniye Ekleyerek Oluşturma

```js
{
  $dateFromParts: {
    year: 2025,
    month: 7,
    day: 14,
    hour: 13,
    minute: 45,
    second: 30
  }
}
```

📌 Çıktı:

```json
ISODate("2025-07-14T13:45:30Z")
```

---

## 🧪 3. Dinamik Kullanım (alanlardan tarih oluştur)

```js
db.kullanicilar.aggregate([
  {
    $project: {
      dogumTarihi: {
        $dateFromParts: {
          year: "$dogumYili",
          month: "$dogumAyi",
          day: "$dogumGunu"
        }
      }
    }
  }
])
```

---

## 🧪 4. Saat Dilimi Belirleme

```js
{
  $dateFromParts: {
    year: 2025,
    month: 7,
    day: 14,
    hour: 9,
    timezone: "Europe/Istanbul"
  }
}
```

---

## 🔁 Ek Özellik: `$isoDateFromParts`

MongoDB 6.0+ sürümünde **ISO haftalarına göre** tarih oluşturmak istersen şu operatörü de kullanabilirsin:

```js
{
  $isoDateFromParts: {
    isoWeekYear: 2025,
    isoWeek: 30,
    isoDayOfWeek: 1 // Pazartesi
  }
}
```

---

## ⚠️ Dikkat Edilecekler

|Konu|Açıklama|
|---|---|
|`year`, `month`, `day` zorunlu|Diğerleri opsiyoneldir|
|Ay değeri 1–12|Ocak = 1|
|Saat dilimi UTC'dir|`timezone` ile değiştirebilirsin|
|Eksik parçalar 0 varsayılır|Örn: `hour` girilmezse 0 olur|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dateFromParts`|Parçalanmış tarih bileşenlerinden Date üretir|
|Dinamik veya sabit değer alır|✅|
|Saat dilimi ayarlanabilir|`"timezone"` opsiyoneldir|
|Yıl/ay/gün zorunludur|En az bu 3'ü gerekir|

---

Sıradaki tarih operatörü gelsin mi TkMatE?

- 🔠 `$dateToString` → Tarihi stringe çevir
    
- 📆 `$dateTrunc` → Tarihi örneğin haftaya/aya yuvarla
    
- 📦 `$dateFromString` → String veriyi Date’e dönüştür
    
- ➖ `$dateSubtract` → Tarihten süre çıkar
    

Hangisi gelsin kral?