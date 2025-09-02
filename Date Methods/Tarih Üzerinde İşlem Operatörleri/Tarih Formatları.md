
Bu formatlar, bir `Date` nesnesini **stringe dönüştürürken** veya **stringi Date’e çevirirken** kullanılır.  
Genellikle şu iki operatörde geçerlidir:

- ✅ `$dateToString`
- ✅ `$dateFromString`

---

## 📆 Temel Tarih Format Kodları

|Format Kodu|Açıklama|Örnek Çıktı|
|---|---|---|
|`%Y`|Yıl (4 haneli)|`2025`|
|`%y`|Yıl (son 2 hane)|`25`|
|`%m`|Ay (01–12)|`07`|
|`%d`|Gün (01–31)|`14`|

---

## 🕰️ Zaman Format Kodları

|Format Kodu|Açıklama|Örnek Çıktı|
|---|---|---|
|`%H`|Saat (00–23)|`15`|
|`%I`|Saat (01–12) – 12'lik|`03`|
|`%M`|Dakika (00–59)|`45`|
|`%S`|Saniye (00–59)|`33`|
|`%L`|Milisaniye (000–999)|`123`|
|`%p`|AM/PM|`PM`|

---

## 📅 Gelişmiş Tarih Format Kodları

|Format Kodu|Açıklama|Örnek|
|---|---|---|
|`%j`|Yılın kaçıncı günü (001–366)|`195`|
|`%U`|Haftanın numarası (Pazar başlangıç)|`28`|
|`%W`|Haftanın numarası (Pazartesi başlangıç)|`28`|
|`%w`|Haftanın günü (0=Pazar, 6=Cumartesi)|`1` (Pazartesi)|
|`%a`|Kısa gün adı (Mon, Tue...)|`Mon`|
|`%A`|Uzun gün adı (Monday, ...)|`Monday`|
|`%b`|Kısa ay adı (Jan, Feb...)|`Jul`|
|`%B`|Uzun ay adı (January, ...)|`July`|

---

## 🎯 Kullanım Örnekleri

### ➤ 1. `dateToString` ile

```js
{
  $dateToString: {
    date: "$tarih",
    format: "%d.%m.%Y %H:%M:%S"
  }
}
```

Çıktı: `"14.07.2025 15:45:33"`

---

### ➤ 2. `dateFromString` ile

```js
{
  $dateFromString: {
    dateString: "14-07-2025 15:45",
    format: "%d-%m-%Y %H:%M"
  }
}
```

---

## ⚠️ Dikkat Edilecekler

- 📌 Format kodları büyük–küçük harf duyarlıdır (`%Y` ≠ `%y`)
    
- 📌 Sıralama önemlidir. Format ve tarih dizgesi aynı yapıda olmalı.
    
- 📌 Yanlış format kodu verirseniz hata alırsınız veya `null` döner.
    
- 📌 Saat ve tarihleri UTC yerine `"timezone"` belirterek yerelleştirebilirsin.
    

---

## ✅ Özet Tablolar

### 🔢 Sayısal Kısaltmalar

|`%Y`|`%m`|`%d`|`%H`|`%M`|`%S`|`%L`|
|---|---|---|---|---|---|---|
|2025|07|14|15|30|22|123|

### 🧠 Bilgilendirici Kısaltmalar

|`%a`|`%A`|`%b`|`%B`|`%p`|
|---|---|---|---|---|
|Mon|Monday|Jul|July|PM|

---

İstersen bu formatlarla ilgili alıştırma soruları, hata senaryoları ya da ileri düzey tarih şablonu örnekleri de oluşturabilirim TkMatE.

✅ Şimdi sırada ne var kral?

- `Conditional` (şart blokları)
    
- `String` operatörleri
    
- `Array` operatörleri
    
- `Control flow`: `$switch`, `$cond`, `$ifNull`
    

Hangisine geçiyoruz?