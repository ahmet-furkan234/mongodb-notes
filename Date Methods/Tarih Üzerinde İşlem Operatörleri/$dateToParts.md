
`$dateToParts`, bir `Date` nesnesini **yıl, ay, gün, saat, dakika, saniye, milisaniye gibi parçalara ayırır** ve bu değerleri içeren bir nesne döner.

---

## 📌 Söz Dizimi

```js
{
  $dateToParts: {
    date: <tarih>,
    timezone: "<isteğe bağlı>",
    iso8601: <boolean> // ISO formatına göre parçalasın mı?
  }
}
```

---

## 📌 Dönen Nesne (Varsayılan UTC Zamanında):

```json
{
  "year": 2025,
  "month": 7,
  "day": 14,
  "hour": 15,
  "minute": 30,
  "second": 45,
  "millisecond": 0,
  "isoWeekYear": 2025,
  "isoWeek": 29,
  "isoDayOfWeek": 1,
  "dayOfYear": 195
}
```

---

## 🧪 1. Temel Kullanım – Tarihi Parçalara Ayırma

```js
db.loglar.aggregate([
  {
    $project: {
      tarihParcalari: {
        $dateToParts: {
          date: "$tarih"
        }
      }
    }
  }
])
```

📌 Çıktı örneği:

```json
{
  tarihParcalari: {
    year: 2025,
    month: 7,
    day: 14,
    hour: 12,
    minute: 0,
    second: 0,
    millisecond: 0,
    ...
  }
}
```

---

## 🧪 2. `new Date()` ile Anlık Zamanı Parçalama

```js
db.test.aggregate([
  {
    $project: {
      simdiParcala: {
        $dateToParts: {
          date: new Date()
        }
      }
    }
  }
])
```

---

## 🧪 3. Saat Dilimi Belirtme

```js
{
  $dateToParts: {
    date: "$tarih",
    timezone: "Europe/Istanbul"
  }
}
```

---

## 🧪 4. ISO 8601 Modu Açıkken (Haftaya göre yıl bilgileri)

```js
{
  $dateToParts: {
    date: "$tarih",
    iso8601: true
  }
}
```

Böylece `isoWeek`, `isoDayOfWeek`, `isoWeekYear` gibi alanlar aktif olur.

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`date` zorunlu|`Date` formatında olmalı|
|Saat dilimi UTC|`"timezone"` ile değiştirilebilir|
|`iso8601: true` dersen ekstra alanlar gelir|ISO haftaları vs.|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dateToParts`|Bir tarihi bileşenlerine ayırır|
|Yıl, ay, gün, saat vb.|Tek tek erişilebilir hale getirir|
|Saat dilimi desteği|`"timezone"` ile kullanılabilir|
|ISO 8601 desteği|`isoWeek`, `isoDayOfWeek`, `isoWeekYear` eklenir|

---

Sırada ne gelsin TkMatE?

- 🔠 `$dateToString` → Tarihi biçimlendirilmiş stringe çevir
    
- 📆 `$dateTrunc` → Tarihi belirli bir zaman dilimine yuvarla
    

Hangisiyle devam edelim ustam?