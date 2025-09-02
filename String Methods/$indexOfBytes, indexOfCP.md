
|Operatör|Açıklama|
|---|---|
|`$indexOfBytes`|Aranan stringin **byte bazlı** ilk pozisyonunu verir (0’dan başlar)|
|`$indexOfCP`|Aranan stringin **karakter bazlı** ilk pozisyonunu verir (0’dan başlar)|

---

## 📌 Söz Dizimi

```js
{ $indexOfBytes: [ <string>, <substring>, <start>, <end> ] }
```

```js
{ $indexOfCP: [ <string>, <substring>, <start>, <end> ] }
```

- `<start>` ve `<end>` opsiyoneldir (arama aralığını belirtir)
- Eğer `<substring>` bulunamazsa `-1` döner

---

## 🧪 1. Basit ASCII Örnek

```js
{
  $indexOfBytes: [ "MongoDB", "DB" ]
}
```

📌 Çıktı: `4` (4. byte'da başlıyor)

```js
{
  $indexOfCP: [ "MongoDB", "DB" ]
}
```

📌 Çıktı: `4` (4. karakterde başlıyor)

---

## 🧪 2. Türkçe Karakterli Metin

```js
{
  $indexOfBytes: [ "Çalışkan", "ış" ]
}
```

- `"ış"` karakterleri byte bazında farklı uzunlukta.
    
- Pozisyon byte olarak döner.
    

```js
{
  $indexOfCP: [ "Çalışkan", "ış" ]
}
```

- Pozisyon karakter bazlıdır.
    
- Daha doğru sonuç verir.
    

---

## 🧪 3. Başlangıç ve Bitiş İndeksleri ile Arama

```js
{
  $indexOfCP: [ "Merhaba Dünya", "a", 5, 12 ]
}
```

- `"a"` karakteri 5–12 aralığında aranır.
    
- Dönen sonuç, arama aralığında ilk eşleşmenin pozisyonudur.
    

---

## 🧪 4. Bulunamayan Arama

```js
{
  $indexOfCP: [ "MongoDB", "XYZ" ]
}
```

📌 Çıktı: `-1` (bulunamadı)

---

## ⚠️ Dikkat Edilecek Noktalar

|Durum|Açıklama|
|---|---|
|`null` girilirse|Sonuç da `null` olur|
|Sayı girilirse|Hata verir, string olmalı|
|Unicode karakterlerde fark|`$indexOfCP` karakter bazlı, daha doğru|
|Başlangıç ve bitiş indeksleri opsiyonel|Varsa arama sınırlandırılır|

---

## ✅ Özet

|Operatör|Ne Zaman Kullanılır?|
|---|---|
|`$indexOfBytes`|Byte bazlı pozisyon lazım ise|
|`$indexOfCP`|Karakter bazlı pozisyon lazım ise, Unicode destekli|

---

## 🧠 Örnek Birlikte Kullanım

```js
db.kullanicilar.aggregate([
  {
    $project: {
      indexByte: { $indexOfBytes: ["$ad", "ş"] },
      indexChar: { $indexOfCP: ["$ad", "ş"] }
    }
  }
])
```

---

Sonraki hangi string operatörü ile devam edelim TkMatE?

- 🔎 `$regexMatch`, `$regexFind`, `$regexFindAll` — Regex ile arama
    
- 🔁 Karma string işlemleri: `$concat`, `$replaceAll`, `$split` vs
    
- 🧪 Uygulamalı örnekler
    

Seçim sende!