
`$substrBytes`, string'in içinden verilen byte başlangıç pozisyonundan itibaren belirtilen byte sayısı kadar keser ve döndürür.

---

## 📌 Söz Dizimi

```js
{
  $substrBytes: [ <input_string>, <start_byte>, <byte_length> ]
}
```

|Parametre|Açıklama|
|---|---|
|`input_string`|Kesilecek metin|
|`start_byte`|Başlangıç byte pozisyonu (0'dan başlar)|
|`byte_length`|Alınacak byte sayısı|

---

## 🧪 1. Basit ASCII Metin (tek baytlı)

```js
{
  $substrBytes: ["MongoDB", 0, 4]
}
```

📌 Çıktı: `"Mongo"`

---

## 🧪 2. Türkçe Karakterli Metin (çok baytlı!)

```js
{
  $substrBytes: ["Çalışkan", 0, 3]
}
```

📌 Çıktı: **Bozuk karakter** olabilir çünkü `"Ç"` → 2 byte

> Bu nedenle `"Ç"` karakteri yarım kesilmiş olabilir. Risklidir.

✅ Alternatif: `$substrCP` kullan — çünkü bu karakter bazlıdır.

---

## 🧪 3. Alan Üzerinde Kullanım

```js
db.kullanicilar.aggregate([
  {
    $project: {
      ilk5Byte: {
        $substrBytes: ["$ad", 0, 5]
      }
    }
  }
])
```

📌 `"Ahmet"` → `"Ahmet"`  
📌 `"Şule"` → `"�"` gibi bozulabilir çünkü `"Ş"` = 2 byte

---

## 🧪 4. Emojili Metin

```js
{
  $substrBytes: ["💡MongoDB", 0, 2]
}
```

📌 Emojinin sadece bir kısmı alınırsa bozulur. Örn: `"�M"` gibi.

---

## ⚠️ Dikkat Edilecek Noktalar

|Uyarı|Açıklama|
|---|---|
|Unicode karakterler (ç, ğ, emoji) çok baytlıdır|`$substrBytes` ile tam kesemezsin|
|Sonuç bozuk karakter olabilir|✅ `$substrCP` ile değiştir|
|Byte sayısı karakter sayısından farklıdır|`"ç"` = 2 byte, `"A"` = 1 byte|
|`null` varsa sonuç da `null` olur|✔️|

---

## ✅ Ne Zaman Kullanılır?

- 🟢 Verinin **tamamı İngilizce/ASCII** karakterlerden oluşuyorsa
    
- 🔴 Unicode karakterli verilerde **tercih edilmez**
    
- 🧪 Byte düzeyinde veri manipülasyonu gerekiyorsa (örneğin sabit uzunlukta binary veri kesmeleri)
    

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$substrBytes`|Byte üzerinden string keser|
|Bozuk karakter riski|Unicode içerikte yüksek|
|Daha güvenli alternatif|`$substrCP` (codepoint bazlı)|

---

Hazırsan sırada:

- 🔤 `$strLenBytes` ve `$strLenCP` – uzunluk ölçme
    
- 🔍 `$indexOfBytes` ve `$indexOfCP` – pozisyon bulma
    
- 🔠 `$regexFind`, `$regexMatch`, `$regexFindAll` – regex ile arama
    
- 🔁 Uygulamalı karma string operasyonları
    

Hangisi gelsin ustam?