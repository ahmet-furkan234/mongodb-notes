
`$strLenCP`, bir string ifadedeki **karakter (Unicode code point)** sayısını döndürür.

> Yani `"ç"` veya `"🧠"` gibi çok baytlı karakterler **tek bir karakter** olarak sayılır.

---

## 📌 Söz Dizimi

```js
{ $strLenCP: <string> }
```

---

## 🧪 1. Basit ASCII Metin

```js
{ $strLenCP: "Mongo" }
```

📌 Sonuç: `5`  
(her harf 1 karakter, 1 byte = 1 karakter burada)

---

## 🧪 2. Türkçe Karakterli Metin

```js
{ $strLenCP: "Çalışkan" }
```

- `"Ç"` = 1 karakter
    
- `"ı"` = 1 karakter
    
- `"ş"` = 1 karakter
    

📌 Toplam: **8 karakter**

---

## 🧪 3. Emoji İçeren Metin

```js
{ $strLenCP: "🧠Bilgi" }
```

- `"🧠"` = 1 karakter
    
- `"Bilgi"` = 5 karakter
    

📌 Toplam: `6`

---

## 🧪 4. Alan Üzerinden Kullanım

```js
db.kullanicilar.aggregate([
  {
    $project: {
      karakterSayisi: {
        $strLenCP: "$ad"
      }
    }
  }
])
```

---

## 🧪 5. Karşılaştırmalı Örnek

```js
{
  $project: {
    cpLength: { $strLenCP: "$ad" },
    byteLength: { $strLenBytes: "$ad" }
  }
}
```

|ad|`$strLenCP`|`$strLenBytes`|
|---|---|---|
|"Ahmet"|5|5|
|"Şule"|4|6|
|"🧠Test"|5|8|

---

## ⚠️ Dikkat Edilecek Noktalar

|Durum|Açıklama|
|---|---|
|`null` girilirse|Sonuç da `null` olur|
|Sayısal değer girilirse|Hata alırsın, önce `$toString` kullan|
|Byte yerine karakter sayısı verir|✅ doğru olan budur|

---

## ✅ Ne Zaman Kullanılmalı?

|Durum|Operatör|
|---|---|
|Metin kaç **karakter** uzunlukta?|✅ `$strLenCP`|
|Metin kaç **byte** uzunlukta?|🔸 `$strLenBytes`|

---

## 🧠 Özet

|Özellik|Açıklama|
|---|---|
|`$strLenCP`|String’in **karakter sayısını** verir|
|Unicode ve emoji desteği|Vardır, doğru sayar|
|Türkçe için güvenli tercih|Evet ✔️|

---

Şimdi sırada hangisi gelsin TkMatE?

- 🔍 `$indexOfCP` / `$indexOfBytes` – aranan alt metnin pozisyonunu bulma
    
- 🔎 `$regexMatch`, `$regexFind`, `$regexFindAll` – regex ile arama
    
- 🔁 `$concat`, `$split`, `$replaceAll` gibi zincirleme örnekler
    
- 🧪 Uygulamalı string işlemleriyle karmaşık senaryolar
    

Karar senin, üstat?