
`$dateSubtract`, bir tarih değerinden yıl, ay, gün, saat gibi zaman birimlerini çıkarır.

---

## 📌 Söz Dizimi

```js
{
  $dateSubtract: {
    startDate: <tarih>,
    unit: "<zaman birimi>",
    amount: <çıkarılacak_sayı>,
    timezone: "<isteğe bağlı>"
  }
}
```

---

## ⏱️ Geçerli `unit` Değerleri

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

## 🧪 1. Temel Kullanım – Tarihten 7 Gün Geri Git

```js
db.loglar.aggregate([
  {
    $project: {
      orijinalTarih: "$tarih",
      yediGunOnce: {
        $dateSubtract: {
          startDate: "$tarih",
          unit: "day",
          amount: 7
        }
      }
    }
  }
])
```

---

## 🧪 2. `new Date()` Üzerinden Saat Geri Gitme

```js
db.now.aggregate([
  {
    $project: {
      simdi: new Date(),
      birSaatOnce: {
        $dateSubtract: {
          startDate: new Date(),
          unit: "hour",
          amount: 1
        }
      }
    }
  }
])
```

---

## 🧪 3. Dinamik Süre Çıkarma – Kullanıcının Üyelik Başlangıcından Süre Silme

```js
db.kullanicilar.aggregate([
  {
    $project: {
      uyeBaslangic: "$kayitTarihi",
      uyelikBitis: {
        $dateSubtract: {
          startDate: "$kayitTarihi",
          unit: "month",
          amount: "$denemeSuresiAy" // örn: 1, 3, 6 gibi
        }
      }
    }
  }
])
```

---

## 🧪 4. Saat Dilimi ile Kullanım

```js
{
  $dateSubtract: {
    startDate: "$tarih",
    unit: "day",
    amount: 3,
    timezone: "Europe/Istanbul"
  }
}
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`startDate` zorunlu|`Date` türünde olmalı|
|`unit` geçerli bir zaman birimi olmalı|`"day"`, `"month"` vs.|
|`amount` pozitif sayı olmalı|Ne kadar geri gidileceğini belirler|
|`timezone` opsiyonel|Varsayılan UTC|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dateSubtract`|Belirtilen süre kadar geçmişe gider|
|`unit` + `amount`|Ne kadar geriye gidileceğini belirler|
|Saat dilimi desteği|`"timezone"` ile ayarlanabilir|
|`$dateAdd` ile zıt mantıkta çalışır|✅|
