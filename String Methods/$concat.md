
`$concat`, verilen string ifadeleri sırayla **uç uca ekler**.  
`null` içeren herhangi bir parça varsa **sonuç da `null` olur!**

---

## 📌 Söz Dizimi

```js
{ $concat: [ <string1>, <string2>, ..., <stringN> ] }
```

Tüm elemanlar **string olmalı ya da stringe çevrilebilir** (`$toString` ile).

---

## 🧪 1. Sabit Değerlerle Kullanım

```js
db.test.aggregate([
  {
    $project: {
      tamIsim: {
        $concat: ["Ahmet", " ", "Yılmaz"]
      }
    }
  }
])
```

📌 Çıktı: `"Ahmet Yılmaz"`

---

## 🧪 2. Alanlar Arası Birleştirme

```js
db.kullanicilar.aggregate([
  {
    $project: {
      tamIsim: {
        $concat: ["$ad", " ", "$soyad"]
      }
    }
  }
])
```

📌 `"Ali Kaya"`

---

## 🧪 3. Sayı Alanı ile Birleştirme (`$toString` ile)

```js
db.urunler.aggregate([
  {
    $project: {
      kod: {
        $concat: ["Kod-", { $toString: "$urunID" }]
      }
    }
  }
])
```

📌 `"Kod-1253"`

---

## 🧪 4. Null Değerin Etkisi

```js
{
  $concat: ["Merhaba ", null, " kullanıcı!"]
}
```

📌 Sonuç: `null`  
➡️ **Çözüm:** `{$ifNull: ["$ad", ""]}` gibi yedek kullanmak

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`null` varsa sonuç da `null` olur|İstisna!|
|Sayı, tarih gibi türler hata verir|`$toString` ile stringe çevir|
|Boş string (`""`) geçerli bir değerdir|Sorun çıkarmaz|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$concat`|String birleştirme operatörüdür|
|Tüm elemanlar string olmalı|ya da `$toString` ile dönüştür|
|`null` varsa sonuç da null|dikkat et!|
