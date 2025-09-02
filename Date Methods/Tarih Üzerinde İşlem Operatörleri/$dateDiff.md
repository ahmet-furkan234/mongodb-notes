
$dateDiff`, iki tarih arasındaki farkı istediğin zaman birimi cinsinden verir:

- gün, ay, yıl, saat, dakika, saniye vs.

---

## 📌 Söz Dizimi

```js
{
  $dateDiff: {
    startDate: <tarih1>,
    endDate: <tarih2>,
    unit: "<zaman birimi>",
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

## 🧪 1. Temel Kullanım – İki Tarih Arasındaki Gün Sayısı

```js
db.loglar.aggregate([
  {
    $project: {
      baslangic: "$baslangicTarihi",
      bitis: "$bitisTarihi",
      farkGun: {
        $dateDiff: {
          startDate: "$baslangicTarihi",
          endDate: "$bitisTarihi",
          unit: "day"
        }
      }
    }
  }
])
```

---

## 🧪 2. `new Date()` ile Bugüne Kadar Geçen Süre (Ay cinsinden)

```js
db.kullanicilar.aggregate([
  {
    $project: {
      kayitTarihi: 1,
      kayitSuresiAy: {
        $dateDiff: {
          startDate: "$kayitTarihi",
          endDate: new Date(),
          unit: "month"
        }
      }
    }
  }
])
```

---

## 🧪 3. Kullanıcı Girişi ile Oturum Süresi (Dakika)

```js
db.oturumlar.aggregate([
  {
    $project: {
      baslangic: "$girisZamani",
      bitis: "$cikisZamani",
      oturumDakika: {
        $dateDiff: {
          startDate: "$girisZamani",
          endDate: "$cikisZamani",
          unit: "minute"
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
  $dateDiff: {
    startDate: "$tarih1",
    endDate: "$tarih2",
    unit: "day",
    timezone: "Europe/Istanbul"
  }
}
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`startDate` ve `endDate` zorunlu|`Date` formatında olmalı|
|`unit` string olmalı|`"day"`, `"month"`, `"hour"` vs.|
|Negatif sonuç olabilir|Eğer `endDate < startDate`|
|Saat dilimi UTC'dir|Gerekirse `timezone` gir|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dateDiff`|İki tarih arasındaki farkı hesaplar|
|Birimi sen belirlersin|yıl, ay, gün, dakika vs.|
|Saat dilimi destekler|`"timezone"` parametresiyle|
|Negatif sonuç dönebilir|Bitiş tarihi başlangıçtan küçükse|
