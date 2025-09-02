

MongoDB'de **alanın var olup olmadığını** veya **türünü** kontrol etmek istersen bu operatörler devreye girer.

---

## 🔹 1. `$exists` – Belirtilen alan **var mı / yok mu?**

### 📌 Görevi:

Belirli bir alanın **belgede tanımlı olup olmadığını** kontrol eder.

### Sözdizimi:

```js
{ alan: { $exists: true } }   // alan varsa
{ alan: { $exists: false } }  // alan yoksa
```

---

### 🧪 Örnekler:

#### 🎯 1. `notlar` alanı olanları getir:

```js
db.ogrenciler.find({
  notlar: { $exists: true }
})
```

#### 🎯 2. `puanlar` alanı **olmayan** belgeleri getir:

```js
db.ogrenciler.find({
  puanlar: { $exists: false }
})
```

---

### 🔥 Gelişmiş: Hem varsa hem boş değilse (örneğin en az 1 yorum içeren ev ilanları)

```js
db.listings.find({
  reviews: {
    $exists: true,
    $not: { $size: 0 }
  }
})
```

---

## 🔹 2. `$type` – Alanın **veri tipi** kontrolü

### 📌 Görevi:

Belirli bir alanın veri tipi, istediğin türle eşleşiyor mu?

### Sözdizimi:

```js
{ alan: { $type: <veri_tipi> } }
```

### Yaygın Türler:

|Tip Adı|Sayı|Açıklama|
|---|---|---|
|"double"|1|Ondalıklı sayı|
|"string"|2|Metin|
|"object"|3|Obje|
|"array"|4|Dizi|
|"bool"|8|true/false|
|"date"|9|Tarih|
|"null"|10|Null değer|
|"int"|16|Tamsayı|

---

### 🧪 Örnekler:

#### 🎯 1. `yas` alanı tamsayı olanları getir:

```js
db.ogrenciler.find({
  yas: { $type: "int" }
})
```

#### 🎯 2. `puanlar` bir dizi olan belgeleri getir:

```js
db.ogrenciler.find({
  puanlar: { $type: "array" }
})
```

#### 🎯 3. `ortalama` alanı string OLANLARI getir (hatalı kayıtları yakalamak için):

```js
db.ogrenciler.find({
  ortalama: { $type: "string" }
})
```

---

## 🧠 `$type` ile Çoklu Tip

```js
db.ogrenciler.find({
  yas: { $type: ["int", "double"] }
})
```

Bu şekilde hem tamsayı hem ondalıklı değerleri eşleştirebilirsin.

---

## 🎯 Challenge Görevleri – Element Operators

1. `notlar` alanı tanımlı olanları getir
2. `puanlar` alanı tanımlı **olmayanları** getir
3. `yas` alanı bir sayı (int veya double) olanları getir
4. `ortalama` alanı string olanları getir
5. `puanlar` bir dizi olanları getir