
### 📅 1. **Tarih Değerleri Oluşturma**

|Fonksiyon|Açıklama|Örnek|
|---|---|---|
|`ISODate()`|ISO 8601 biçiminde tarih oluşturur|`ISODate("2023-01-01T00:00:00Z")`|
|`new Date()`|JavaScript Date nesnesi gibi çalışır|`new Date("2023-01-01")`|
|`NumberLong()`|Unix epoch time olarak long değer|`new Date(NumberLong("1672531200000"))`|

---

### 🕹️ 2. **Tarih Operatörleri (Aggregation Stage içinde)**

Bu operatörler **`$project`, `$addFields`, `$group`, `$match`** gibi aşamalarda kullanılır:

#### 📆 A. **Tarihten Bileşenleri Çekme**

| Operatör        | Açıklama                             | Örnek                         |
| --------------- | ------------------------------------ | ----------------------------- |
| `$year`         | Yılı döner                           | `{ $year: "$tarih" } → 2025`  |
| `$month`        | Ayı döner (1-12)                     | `{ $month: "$tarih" } → 7`    |
| `$dayOfMonth`   | Gün döner (1-31)                     | `{ $dayOfMonth: "$tarih" }`   |
| `$hour`         | Saati döner (0-23)                   | `{ $hour: "$tarih" }`         |
| `$minute`       | Dakikayı döner                       | `{ $minute: "$tarih" }`       |
| `$second`       | Saniyeyi döner                       | `{ $second: "$tarih" }`       |
| `$millisecond`  | Milisaniye                           | `{ $millisecond: "$tarih" }`  |
| `$dayOfWeek`    | Haftanın günü (1=Pazar, 7=Cumartesi) | `{ $dayOfWeek: "$tarih" }`    |
| `$week`         | Yılın kaçıncı haftası                | `{ $week: "$tarih" }`         |
| `$dayOfYear`    | Yılın kaçıncı günü                   | `{ $dayOfYear: "$tarih" }`    |
| `$isoWeek`      | ISO 8601 haftası                     | `{ $isoWeek: "$tarih" }`      |
| `$isoDayOfWeek` | ISO gün (1=Pazartesi, 7=Pazar)       | `{ $isoDayOfWeek: "$tarih" }` |
| `$isoWeekYear`  | ISO haftasına göre yıl               | `{ $isoWeekYear: "$tarih" }`  |

---

#### 🛠️ B. **Tarih Üzerinde İşlem Yapma**

| Operatör          | Açıklama                               | Örnek                                                                 |
| ----------------- | -------------------------------------- | --------------------------------------------------------------------- |
| `$dateAdd`        | Tarihe zaman ekler                     | `{ $dateAdd: { startDate: "$tarih", unit: "day", amount: 5 }}`        |
| `$dateSubtract`   | Tarihten zaman çıkarır                 | `{ $dateSubtract: { startDate: "$tarih", unit: "month", amount: 1 }}` |
| `$dateDiff`       | İki tarih arasındaki farkı hesaplar    | `{ $dateDiff: { startDate: "$a", endDate: "$b", unit: "day" }}`       |
| `$dateTrunc`      | Tarihi belirtilen birime göre yuvarlar | `{ $dateTrunc: { date: "$tarih", unit: "month" }}`                    |
| `$dateFromParts`  | Parçalardan tarih üretir               | `{ $dateFromParts: { year: 2025, month: 7, day: 14 }}`                |
| `$dateToParts`    | Tarihi parçalara böler                 | `{ $dateToParts: { date: "$tarih", iso8601: true }}`                  |
| `$dateFromString` | String'den tarih oluşturur             | `{ $dateFromString: { dateString: "2024-07-14T00:00:00Z" }}`          |
| `$dateToString`   | Tarihi string'e çevirir                | `{ $dateToString: { date: "$tarih", format: "%Y-%m-%d" }}`            |

---

### 📊 3. **Karşılaştırma Operatörleri (Date ile)**

|Operatör|Açıklama|Örnek|
|---|---|---|
|`$gt`, `$lt`, `$gte`, `$lte`|Tarih karşılaştırması|`{ "createdAt": { $gt: ISODate("2023-01-01") }}`|
|`$eq`, `$ne`|Eşitlik kontrolü|`{ "updatedAt": { $eq: ISODate("2025-07-14") }}`|

---

### 🧮 4. **Tarih Formatlama (String Olarak)**

- `%Y` → Yıl (4 haneli)
- `%m` → Ay (01-12)
- `%d` → Gün
- `%H` → Saat (00-23)
- `%M` → Dakika
- `%S` → Saniye
- `%L` → Milisaniye
- `%j` → Yılın günü

📌 Örnek:

```js
{
  $dateToString: {
    date: "$createdAt",
    format: "%Y-%m-%d %H:%M:%S"
  }
}
```

---

### 🧪 5. **Ekstra Bilgiler**

- Tarih alanları her zaman UTC'ye göre saklanır.
- Tarih karşılaştırmaları milisaniye hassasiyetindedir.
- `ISODate()` kullanımı tarih alanlarında önerilir.

---

### 📘 Örnek Kullanım (Full Aggregation Örneği)

```js
db.orders.aggregate([
  {
    $project: {
      yıl: { $year: "$createdAt" },
      ay: { $month: "$createdAt" },
      haftanınGünü: { $dayOfWeek: "$createdAt" },
      günString: { $dateToString: { date: "$createdAt", format: "%Y-%m-%d" } }
    }
  }
])
```
