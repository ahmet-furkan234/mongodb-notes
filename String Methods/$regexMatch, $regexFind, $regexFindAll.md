
|Operatör|Açıklama|
|---|---|
|`$regexMatch`|Regex deseni ile metnin eşleşip eşleşmediğini boolean olarak döner|
|`$regexFind`|Metindeki ilk eşleşen kısmı ve pozisyonunu verir (dizi formatında)|
|`$regexFindAll`|Metindeki tüm eşleşen kısımları ve pozisyonlarını verir (dizi formatında)|

---

## 📌 Genel Kullanım Söz Dizimi

```js
{
  $regexMatch: {
    input: <string>,
    regex: <regex_pattern>,
    options: <regex_options> // opsiyonel, örn: "i" (ignore case)
  }
}
```

Benzer şekilde `$regexFind` ve `$regexFindAll` da aynı parametre yapısını kullanır.

---

## 🧪 1. `$regexMatch` Örneği

```js
{
  $regexMatch: {
    input: "MongoDB Eğitim",
    regex: "Eğitim"
  }
}
```

📌 Sonuç: `true` (Eğitim kelimesi var)

---

## 🧪 2. `$regexMatch` Case-Insensitive Arama

```js
{
  $regexMatch: {
    input: "MongoDB eğitim",
    regex: "EĞİTİM",
    options: "i"
  }
}
```

📌 Sonuç: `true` (Büyük-küçük harf duyarsız)

---

## 🧪 3. `$regexFind` – İlk Eşleşmeyi Bul

```js
{
  $regexFind: {
    input: "123abc456abc789",
    regex: "abc"
  }
}
```

📌 Çıktı:

```json
{
  "match": "abc",
  "idx": 3,
  "captures": []
}
```

- `match`: bulunan metin
- `idx`: pozisyon (karakter bazlı)
- `captures`: grup varsa içindeki yakalananlar

---

## 🧪 4. `$regexFindAll` – Tüm Eşleşmeleri Bul

```js
{
  $regexFindAll: {
    input: "123abc456abc789",
    regex: "abc"
  }
}
```

📌 Çıktı:

```json
[
  { "match": "abc", "idx": 3, "captures": [] },
  { "match": "abc", "idx": 9, "captures": [] }
]
```

---

## 🧪 5. Grup Yakalama Örneği

```js
{
  $regexFind: {
    input: "abc123def",
    regex: "(abc)(123)"
  }
}
```

📌 Çıktı:

```json
{
  "match": "abc123",
  "idx": 0,
  "captures": ["abc", "123"]
}
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Durum|Açıklama|
|---|---|
|Regex yazımı|JavaScript regex standartlarına uygun olmalı|
|`input` null ise|Sonuç `null` olur|
|Büyük-küçük harf duyarlılığı|`options: "i"` ile kontrol edilir|
|Karmaşık regexler|Performansı etkileyebilir|

---

## ✅ Özet

|Operatör|Dönen Değer|Kullanım Alanı|
|---|---|---|
|`$regexMatch`|`true`/`false`|Metin içinde eşleşme var mı?|
|`$regexFind`|İlk eşleşen metin ve pozisyon|Eşleşen ilk bölümü detaylı almak|
|`$regexFindAll`|Tüm eşleşmelerin listesi|Çoklu eşleşmeler için|

---

Şimdi sıradaki hangi string operatörü olsun TkMatE?

- 📏 `$strLenCP`, `$indexOfCP` tekrar kısa özet
    
- 🔁 Karma string işlemleri: `$concat`, `$split`, `$replaceAll` vb.
    
- 🧪 Uygulamalı string örnekleri (birleştir, böl, temizle vs)
    
- 🔎 MongoDB Array operatörlerine geçiş
    

Sen seç, devam edelim!