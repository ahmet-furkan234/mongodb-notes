
`$dateAdd`, bir tarih alanına gün, ay, yıl, saat gibi zaman birimlerini **eklemek** için kullanılır.

---

## 📌 Söz Dizimi

```js
{
  $dateAdd: {
    startDate: <tarih>,
    unit: "<zaman birimi>",
    amount: <eklenecek sayı>,
    timezone: "<isteğe bağlı>"
  }
}
```

---

## 🕐 Geçerli `unit` Değerleri

|Unit Değeri|Anlamı|
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

## 🧪 1. Temel Kullanım – Tarihe 5 Gün Ekleme

```js
db.loglar.aggregate([
  {
    $project: {
      orijinalTarih: "$tarih",
      yeniTarih: {
        $dateAdd: {
          startDate: "$tarih",
          unit: "day",
          amount: 5
        }
      }
    }
  }
])
```

---

## 🧪 2. `new Date()` ile Şu Anın Üzerine Ekleme

```js
db.rapor.aggregate([
  {
    $project: {
      simdi: new Date(),
      birSaatSonra: {
        $dateAdd: {
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

## 🧪 3. Dinamik Kullanım – Kullanıcı girdisine göre dakika ekleme

```js
db.oturumlar.aggregate([
  {
    $project: {
      baslangic: "$loginDate",
      bitis: {
        $dateAdd: {
          startDate: "$loginDate",
          unit: "minute",
          amount: "$oturumSuresi"
        }
      }
    }
  }
])
```

---

## 🧪 4. Saat dilimi kullanımı (opsiyonel)

```js
{
  $dateAdd: {
    startDate: "$tarih",
    unit: "day",
    amount: 1,
    timezone: "Europe/Istanbul"
  }
}
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`startDate` zorunludur|`Date` tipinde olmalı|
|`unit` string olmalı|`"day"`, `"month"` vb.|
|`amount` pozitif/negatif olabilir|Negatifse tarih geri gider|
|UTC kullanılır|İstersen `timezone` belirt|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dateAdd`|Belirli bir tarihe zaman ekler|
|Esnek birimler|yıl, ay, gün, saat, dakika, saniye|
|Negatif sayı destekler|Geriye doğru tarih için kullanılabilir|
|Saat dilimi desteği|`"timezone"` ile ayarlanabilir|

---

Sıradaki tarih operatörüyle devam edelim mi TkMatE?

- ⏳ `$dateDiff` → İki tarih arasındaki fark
    
- ➖ `$dateSubtract` → Tarihten çıkarma
    
- 🔠 `$dateToString` → Tarihi yazıya çevirme
    
- 📆 `$dateTrunc` → Tarihi örneğin haftaya/aya yuvarlama
    

Hangisi gelsin kral?