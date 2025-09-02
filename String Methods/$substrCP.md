
`$substrCP`, bir string ifadenin içinden **başlangıç karakterinden itibaren belirli sayıda karakteri kesip** yeni bir string döndürür.

> `CP` = Code Point → Yani Unicode karakter bazlı işlem yapar.

---

## 📌 Söz Dizimi

```js
{
  $substrCP: [ <input>, <start>, <length> ]
}
```

|Parametre|Açıklama|
|---|---|
|`<input>`|String ifade (alan adı veya sabit metin)|
|`<start>`|Başlangıç pozisyonu (0'dan başlar)|
|`<length>`|Alınacak karakter sayısı|

---

## 🧪 1. Sabit Metin Üzerinde Kullanım

```js
{ $substrCP: ["MongoDB Öğreniyorum", 0, 7] }
```

📌 Çıktı: `"MongoDB"`

---

## 🧪 2. Alan Üzerinden Kesme

```js
db.kullanicilar.aggregate([
  {
    $project: {
      ilk3Harf: { $substrCP: ["$ad", 0, 3] }
    }
  }
])
```

📌 `"Ahmet"` → `"Ahm"`

---

## 🧪 3. Son Harfleri Alma (dinamik uzunlukla)

```js
db.kullanicilar.aggregate([
  {
    $project: {
      son2Harf: {
        $substrCP: [
          "$ad",
          { $subtract: [ { $strLenCP: "$ad" }, 2 ] },
          2
        ]
      }
    }
  }
])
```

📌 `"Mehmet"` → `"et"`

---

## 🧪 4. Türkçe / Emoji / Unicode Karakterlerle Kullanım

```js
{ $substrCP: ["🧠Bilgi Güçtür", 0, 3] }
```

📌 Çıktı: `"🧠Bi"`

> ❗ **Not:** `$substrBytes` kullansaydık burada sorun çıkardı çünkü `🧠` gibi karakterler çok baytlıdır.

---

## ⚠️ Dikkat Edilecek Noktalar

|Durum|Açıklama|
|---|---|
|`start` negatif olamaz|Hata verir|
|`length` sıfırsa boş string döner|✅|
|`start + length` metin uzunluğunu aşarsa|Uygun kadarı döner|
|`input` null ise|Sonuç `null` olur|
|`input` string değilse|`$toString` ile dönüştürülmeli|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$substrCP`|Unicode karaktere göre kesme|
|Başlangıç + uzunluk|karakter pozisyonuna göre hesaplanır|
|Emojiler, Türkçe karakterler desteklenir|✅|
