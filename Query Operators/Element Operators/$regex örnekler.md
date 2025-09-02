
| Yapmak İstediğin İşlem              | Regex Pattern Örneği      | Açıklama                 |
| ----------------------------------- | ------------------------- | ------------------------ |
| Belirli bir metinle **başlayanlar** | `^abc`                    | "abc" ile başlar         |
| Belirli bir metinle **bitenler**    | `xyz$`                    | "xyz" ile biter          |
| Belirli bir metni **içerenler**     | `test`                    | içinde "test" geçer      |
| Sadece **rakam** içerenler          | `^[0-9]+$`                | 1+ rakam, sadece         |
| Sadece **harf** içerenler           | `^[a-zA-Z]+$`             | Küçük & büyük harf       |
| **Büyük/küçük harf duyarsız** arama | `"ahmet"`, options: `"i"` | "Ahmet", "AHMET" eşleşir |
| Belirli uzunlukta değer kontrolü    | `^.{5}$`                  | 5 karakter olanlar       |

---

## 🔧 Regex Pattern Nasıl Yazılır?

### 📌 Temel Yapılar:

|Karakter|Anlamı|
|---|---|
|`^`|Başlangıç|
|`$`|Bitiş|
|`.`|Herhangi bir karakter (wildcard)|
|`[abc]`|a, b, ya da c|
|`[^abc]`|a, b, c hariç hepsi|
|`[0-9]`|0–9 arası rakam|
|`[a-z]`|Küçük harf|
|`[A-Z]`|Büyük harf|
|`+`|1 veya daha fazla tekrar|
|`*`|0 veya daha fazla tekrar|
|`?`|0 ya da 1 tekrar|
|`{n}`|tam olarak n tekrar|
|`{n,}`|en az n tekrar|
|`{n,m}`|en az n, en çok m tekrar|
|`\.`|Nokta karakteri (literal)|
|`\\`|Ters eğik çizgi|

---

## 🧪 MongoDB Regex Kullanımı

### 🎯 1. `ad` alanı "A" ile başlayanlar:

```js
db.ogrenciler.find({
  ad: { $regex: /^A/ }
})
```

### 🎯 2. `email` içinde “gmail” geçenler:

```js
db.ogrenciler.find({
  email: { $regex: /gmail/ }
})
```

### 🎯 3. `telefon` alanı sadece 11 rakamdan oluşanlar:

```js
db.ogrenciler.find({
  telefon: { $regex: /^[0-9]{11}$/ }
})
```

---

## 🎯 4. `soyad` alanı “oğlu” ile bitenler:

```js
db.ogrenciler.find({
  soyad: { $regex: /oğlu$/ }
})
```

---

## 🔥 Büyük/küçük harf fark etmesin?

```js
db.ogrenciler.find({
  ad: { $regex: "ahmet", $options: "i" }  // i = ignore case
})
```

---

## 🎯 Challenge Görevleri:

1. `ad` küçük harfle başlayanları getir
2. `email` adresi `.edu.tr` ile bitenleri getir
3. `tc` alanı sadece 11 rakamdan oluşuyorsa getir
4. `soyad` sadece harflerden oluşuyorsa getir
5. `adres` alanında "istanbul" geçenleri (büyük/küçük fark etmeksizin) getir
