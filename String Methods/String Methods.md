
| Operatör         | Açıklama                                  |
| ---------------- | ----------------------------------------- |
| `$concat`        | String birleştirme                        |
| `$substr` (eski) | Belirli kısmı alma                        |
| `$substrBytes`   | Byte bazlı kesme                          |
| `$substrCP`      | Unicode karakter bazlı kesme              |
| `$toUpper`       | Harfleri büyütme                          |
| `$toLower`       | Harfleri küçültme                         |
| `$trim`          | Hem baştan hem sondan kırpma              |
| `$ltrim`         | Sadece baştaki karakterleri kırpma        |
| `$rtrim`         | Sadece sondaki karakterleri kırpma        |
| `$strLenBytes`   | Byte cinsinden uzunluk                    |
| `$strLenCP`      | Karakter (Unicode) sayısı                 |
| `$indexOfBytes`  | Alt string’in byte bazlı index’i          |
| `$indexOfCP`     | Alt string’in karakter bazlı index’i      |
| `$split`         | Metni ayırma (bölme)                      |
| `$regexFind`     | Regex ile eşleşme bulma                   |
| `$regexFindAll`  | Regex ile tüm eşleşmeleri bulma           |
| `$regexMatch`    | Regex ile eşleşme olup olmadığını kontrol |
| `$replaceOne`    | İlk eşleşeni değiştir                     |
| `$replaceAll`    | Tüm eşleşmeleri değiştir                  |

---

## 🔹 1. `$concat` – Stringleri birleştirir

```js
{
  $concat: ["Merhaba ", "$ad", " ", "$soyad"]
}
```

📌 `"Merhaba Ahmet Yılmaz"`

---

## 🔹 2. `$substrBytes` – Belirli byte aralığını alır

```js
{
  $substrBytes: ["$isim", 0, 3]
}
```

📌 `"Ahm"` – ilk 3 byte

---

## 🔹 3. `$substrCP` – Unicode karaktere göre keser (emoji uyumlu!)

```js
{
  $substrCP: ["$mesaj", 0, 2]
}
```

📌 `"😊👍"` → ilk 2 karakter

---

## 🔹 4. `$toUpper` – BÜYÜK harfe çevirir

```js
{ $toUpper: "$ad" } // "AHMET"
```

---

## 🔹 5. `$toLower` – küçük harfe çevirir

```js
{ $toLower: "$email" } // "ahmet@example.com"
```

---

## 🔹 6. `$trim` – Baş ve son boşluk/karakter silme

```js
{
  $trim: {
    input: "  MongoDB  ",
    chars: " " // varsayılan boşluk
  }
}
```

📌 `"MongoDB"`

---

## 🔹 7. `$ltrim` / `$rtrim` – Sadece başı veya sonu kırpar

```js
{ $ltrim: { input: "  test ", chars: " " } } → `"test "`
{ $rtrim: { input: "  test ", chars: " " } } → `"  test"`
```

---

## 🔹 8. `$strLenBytes` – Byte cinsinden uzunluk

```js
{ $strLenBytes: "$ad" }
```

📌 `"ç"` → 2 byte, `"A"` → 1 byte

---

## 🔹 9. `$strLenCP` – Karakter (Unicode) uzunluğu

```js
{ $strLenCP: "$ad" } // "ç" de 1 sayılır
```

---

## 🔹 10. `$indexOfBytes` / `$indexOfCP` – Alt string’in pozisyonu

```js
{
  $indexOfCP: {
    input: "mongodb öğreniyorum",
    find: "öğren"
  }
}
```

📌 `8`

---

## 🔹 11. `$split` – Stringi parçalar (array'e dönüştürür)

```js
{
  $split: ["Ahmet,Mehmet,Ayşe", ","]
}
```

📌 `["Ahmet", "Mehmet", "Ayşe"]`

---

## 🔹 12. `$regexFind` – İlk regex eşleşmesini döndürür

```js
{
  $regexFind: {
    input: "$metin",
    regex: /[A-Z]{3}/
  }
}
```

📌 `{ match: "ABC", idx: 10, captures: [] }`

---

## 🔹 13. `$regexFindAll` – Tüm eşleşmeleri döndürür

```js
{
  $regexFindAll: {
    input: "$metin",
    regex: /[A-Z]{3}/g
  }
}
```

---

## 🔹 14. `$regexMatch` – Eşleşme olup olmadığını boolean verir

```js
{
  $regexMatch: {
    input: "$email",
    regex: /^[\w\-\.]+@gmail\.com$/
  }
}
```

📌 `true` veya `false`

---

## 🔹 15. `$replaceOne` – İlk eşleşen kısmı değiştirir

```js
{
  $replaceOne: {
    input: "MongoDB Öğren MongoDB",
    find: "MongoDB",
    replacement: "🔥Mongo"
  }
}
```

📌 `"🔥Mongo Öğren MongoDB"`

---

## 🔹 16. `$replaceAll` – Tüm eşleşenleri değiştirir

```js
{
  $replaceAll: {
    input: "MongoDB Öğren MongoDB",
    find: "MongoDB",
    replacement: "🔥Mongo"
  }
}
```

📌 `"🔥Mongo Öğren 🔥Mongo"`

---

## ✅ Özetle

|Kategori|Operatörler|
|---|---|
|🔠 Harf Biçimi|`$toUpper`, `$toLower`|
|🔪 Kesme/Kırpma|`$substrCP`, `$trim`, `$split`|
|🔍 Arama|`$regexFind`, `$regexMatch`, `$indexOfCP`|
|🧮 Uzunluk|`$strLenCP`, `$strLenBytes`|
|🔁 Değiştirme|`$replaceOne`, `$replaceAll`|

---

Hazırsan bunların her birini **örnek veri setleri üzerinde uygulamalı olarak** tek tek işleyebiliriz.

Sıradaki adım:

- ➕ `$concat`, `$toUpper`, `$split`, `$replaceAll` gibi örneklerle başlayalım mı?
    
- Yoksa `$regexFind`, `$regexMatch` gibi **regex bazlı** operatörlere mi geçelim?
    

Sen seç TkMatE!