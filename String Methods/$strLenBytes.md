
`$strLenBytes`, bir string ifadenin **kaç byte uzunluğunda** olduğunu döndürür.

> Önemli: Bu, karakter sayısı değil, **byte** sayısıdır.  
> Her karakter = 1 byte değildir.  
> Örneğin: `"ç"` → **2 byte**, `"A"` → **1 byte**, `"🧠"` → **4 byte**

---

## 📌 Söz Dizimi

```js
{ $strLenBytes: <string> }
```

---

## 🧪 1. Basit ASCII Metin

```js
{ $strLenBytes: "Mongo" }
```

📌 `"Mongo"` → 5 byte (her harf 1 byte)

---

## 🧪 2. Türkçe Karakterli Metin

```js
{ $strLenBytes: "Çalışkan" }
```

- `"Ç"` = 2 byte
    
- `"a"` = 1 byte
    
- `"l"` = 1 byte
    
- `"ı"` = 2 byte
    
- `"ş"` = 2 byte
    
- `"k"` = 1 byte
    
- `"a"` = 1 byte
    
- `"n"` = 1 byte
    

📌 Toplam: **11 byte**

---

## 🧪 3. Emoji İçeren Metin

```js
{ $strLenBytes: "🧠Bilgi" }
```

- `"🧠"` = 4 byte
    
- `"Bilgi"` = 5 byte
    

📌 Toplam: **9 byte**

---

## 🧪 4. Alan Üzerinde Kullanım

```js
db.kullanicilar.aggregate([
  {
    $project: {
      byteUzunluk: {
        $strLenBytes: "$ad"
      }
    }
  }
])
```

---

## 💡 Karşılaştırma: `$strLenCP` vs `$strLenBytes`

|Girdi|`$strLenCP` (Karakter Sayısı)|`$strLenBytes` (Byte Sayısı)|
|---|---|---|
|`"Ahmet"`|5|5|
|`"Çalış"`|5|8|
|`"🧠Test"`|5|8|

---

## ⚠️ Dikkat Edilecek Noktalar

|Uyarı|Açıklama|
|---|---|
|Unicode karakterler 1’den fazla byte|Örn: `ç`, `ğ`, `ö`, emoji|
|`null` girilirse|Sonuç da `null` olur|
|Sayı girersen|Hata verir, string beklenir|
|Güvenli alternatif|Karakter sayısı gerekiyorsa: `$strLenCP` ✅|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$strLenBytes`|String'in **byte cinsinden** uzunluğu|
|Dikkat edilmesi gereken durum|Unicode karakterler ve emojiler|
|Karakter sayısı gerekirse|`$strLenCP` kullanılmalı|

---

Sıradaki adımı sen seç TkMatE:

- 🔢 `$strLenCP` → karakter uzunluğu
    
- 🔍 `$indexOfBytes` veya `$indexOfCP` → aranan metnin pozisyonu
    
- 🔠 `$regexFind`, `$regexMatch`, `$regexFindAll` → regex ile metin arama
    
- 🔁 Uygulamalı string işleme örnekleri (birleştir, böl, değiştir, normalize)
    

Hangisi gelsin üstadım?