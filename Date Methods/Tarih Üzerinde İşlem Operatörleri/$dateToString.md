
`$dateToString`, bir `Date` nesnesini özel bir **formatta string olarak döner**.  
Özellikle raporlama, etiketleme ve tarih karşılaştırmalarında kullanılır.

---

## 📌 Söz Dizimi

```js
{
  $dateToString: {
    date: <Date>,
    format: "<format_str>",
    timezone: "<isteğe bağlı>",
    onNull: "<null ise dönecek değer>"
  }
}
```

---

## 📆 Yaygın Format Kodları

|Kod|Anlamı|Örnek|
|---|---|---|
|`%Y`|4 haneli yıl|2025|
|`%m`|Ay (01–12)|07|
|`%d`|Gün (01–31)|14|
|`%H`|Saat (00–23)|09|
|`%M`|Dakika (00–59)|45|
|`%S`|Saniye (00–59)|33|
|`%L`|Milisaniye (000–999)|125|
|`%j`|Yılın günü (001–366)|195|
|`%w`|Haftanın günü (0=Pazar, 6=Cumartesi)|1|

---

## 🧪 1. Temel Kullanım – Tarihi `gg.aa.yyyy` Formatında Yazdırma

```js
db.kayitlar.aggregate([
  {
    $project: {
      tarihMetni: {
        $dateToString: {
          date: "$tarih",
          format: "%d.%m.%Y"
        }
      }
    }
  }
])
```

📌 Örnek çıktı:

```json
{ "tarihMetni": "14.07.2025" }
```

---

## 🧪 2. Saat ve Dakika Ekleyerek Formatlama

```js
{
  $dateToString: {
    date: "$tarih",
    format: "%Y-%m-%d %H:%M"
  }
}
```

📌 Örnek çıktı: `"2025-07-14 13:45"`

---

## 🧪 3. `new Date()` ile Şu Anki Tarihi Formatlama

```js
db.test.aggregate([
  {
    $project: {
      suAn: {
        $dateToString: {
          date: new Date(),
          format: "%Y-%m-%d %H:%M:%S"
        }
      }
    }
  }
])
```

---

## 🧪 4. Saat Dilimi Belirleme (Opsiyonel)

```js
{
  $dateToString: {
    date: "$tarih",
    format: "%Y-%m-%d %H:%M",
    timezone: "Europe/Istanbul"
  }
}
```

---

## 🧪 5. Null Değer için Varsayılan Ayarlama

```js
{
  $dateToString: {
    date: "$tarih",
    format: "%Y-%m-%d",
    onNull: "Tarih yok"
  }
}
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`date` zorunlu|`Date` formatında olmalı|
|`format` çok önemlidir|Hatalı format, hatalı string döndürür|
|Saat dilimi UTC'dir|`timezone` ile yerel saat çevrilebilir|
|`onNull` varsa null yerine özel değer döner|Varsayılan string|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$dateToString`|`Date` değerini formatlı stringe dönüştürür|
|`%Y %m %d %H %M` gibi format kodları kullanılır|✅|
|Saat dilimi desteği vardır|`"timezone"` ile çalışır|
|Null kontrolü yapılabilir|`"onNull"` ile|

---

Sırada ne var TkMatE?

- 📆 `$dateTrunc` → Tarihi örneğin aya, haftaya, güne **yuvarlama**
    
- 🔄 `$dateFromParts`, `$dateFromString`, `$dateToParts`, `$dateToString` → Bitti
    

İstersen `$dateTrunc` ile tarihleri gruplama işlemlerine geçebiliriz. Ne dersin?