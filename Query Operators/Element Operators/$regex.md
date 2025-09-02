
### 📌 Görevi:

Belirtilen alanın değeri, verdiğin **düzenli ifadeyle (regex pattern)** eşleşiyor mu, onu kontrol eder.

---

## 🔹 Temel Sözdizimi:

```js
{ alan: { $regex: /desen/ } }

veya

{ alan: { $regex: "desen" } }
```

> İsteğe bağlı olarak `"i"` gibi **modifikasyonlar** da eklenebilir (büyük/küçük harf duyarsızlık gibi).

---

## 🧪 Temel Örnekler

### 🎯 1. `ad` alanı "a" harfiyle başlayan öğrencileri getir:

```js
db.ogrenciler.find({
  ad: { $regex: /^a/ }
})
```

---

### 🎯 2. `ad` içinde "ahmet" geçen öğrencileri getir (büyük/küçük fark etmeden):

```js
db.ogrenciler.find({
  ad: { $regex: "ahmet", $options: "i" }
})
```

Alternatif kısa hali:

```js
db.ogrenciler.find({
  ad: /ahmet/i
})
```

---

### 🎯 3. `email` sonu "edu.tr" olanları getir:

```js
db.ogrenciler.find({
  email: { $regex: /\.edu\.tr$/ }
})
```

---

### 🎯 4. `telefon` sadece rakamlardan oluşanları getir:

```js
db.ogrenciler.find({
  telefon: { $regex: /^[0-9]+$/ }
})
```

---

## 🔄 Sık Kullanılan Regex Desenleri

|Amaç|Regex|Açıklama|
|---|---|---|
|"abc" ile başlayan|`^abc`|Başlangıç kontrolü|
|"xyz" ile biten|`xyz$`|Bitiş kontrolü|
|İçinde "test" geçen|`test`|Geçen yerde eşleşir|
|Sadece sayılar|`^[0-9]+$`|En az bir rakam|
|Harf ve boşluklarla oluşan|`^[a-zA-Z ]+$`|Sadece harf ve boşluk|
|Büyük/küçük harf duyarsız arama|`"desen"`, `"i"`|`$options: "i"` ile yapılır|

---

## ⚠️ Performans Uyarısı

- `$regex` ile başlayan veya içinde arama **index** kullanmaz → **yavaş olabilir.**
- `^` ile başlayan ve sabit desenler **index** kullanabilir.  
    Örn: `{ ad: { $regex: /^A/ } }` hızlıdır.

---

## 🎯 Challenge Görevleri – `$regex`

1. `ad` alanı "m" harfiyle başlayanları getir
2. `email` içinde "gmail" geçenleri getir
3. `ad` sadece harflerden oluşanları getir
4. `telefon` alanı 11 rakamdan oluşuyorsa getir
5. `soyad` "oğlu" ile bitenleri getir
