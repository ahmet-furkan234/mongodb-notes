
Bir belgedeki belirli alanın **veri tipinin** istediğin tür olup olmadığını kontrol etmek için kullanılır.

---

## 🔹 Sözdizimi:

```js
{ alanAdi: { $type: <tip> } }
```

- `<tip>` ya tip ismi (string) ya da tip kodu (sayı) olabilir.
- Çoklu tip kontrolü için tiplerin bir dizisi verilebilir.

---

## 🔢 Yaygın Tipler ve Kodları:

|Tip İsmi|Kod|Açıklama|
|---|---|---|
|"double"|1|Ondalıklı sayı|
|"string"|2|Metin (string)|
|"object"|3|Obje|
|"array"|4|Dizi|
|"bool"|8|Boolean (true/false)|
|"date"|9|Tarih|
|"null"|10|Null değeri|
|"int"|16|Tamsayı|

---

## 🧪 Örnekler:

### 🎯 1. `yas` alanı tamsayı (`int`) olan öğrenciler:

```js
db.ogrenciler.find({
  yas: { $type: "int" }
})
```

### 🎯 2. `puanlar` alanı bir dizi (`array`) olanlar:

```js
db.ogrenciler.find({
  puanlar: { $type: "array" }
})
```

### 🎯 3. `ortalama` alanı string olanlar (hatalı kayıtları bulmak için):

```js
db.ogrenciler.find({
  ortalama: { $type: "string" }
})
```

---

## 🔄 Çoklu Tip Kontrolü

Birden fazla tip için kontrol yapmak istersen, tipleri dizi halinde yazabilirsin:

```js
db.ogrenciler.find({
  yas: { $type: ["int", "double"] }
})
```

Bu sorgu, hem tamsayı hem ondalıklı sayı olanları getirir.

---

## 🎯 Challenge Görevleri – `$type`

1. `notlar` alanı bir obje olanları getir
2. `puanlar` alanı dizi olanları getir
3. `yas` alanı sayı (int veya double) olanları getir
4. `ortalama` alanı string olanları getir
5. `adres` alanı olmayanları getir (`$exists` ile kombinasyon yap)
