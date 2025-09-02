
Belirtilen alanın, belirtilen değere **eşit** olup olmadığını kontrol eder.

---

## 🔹 Temel Sözdizimi

```js
{ alanAdi: { $eq: deger } }
```

> **Aynı şeyin kısaltması:** `{ alanAdi: deger }`  
> Ama `$eq` bazen **değişkenli veya dinamik sorgularda** tercih edilir.

---

## 🧪 Temel Örnekler

### 🎯 1. Yaşı 20 olan öğrenciler:

```js
db.ogrenciler.find({ yas: { $eq: 20 } })
```

### 🎯 2. Ortalaması tam olarak 80 olanlar:

```js
db.ogrenciler.find({ ortalama: { $eq: 80 } })
```

### 🎯 3. Matematik notu 85 olanlar (`notlar.mat`)

```js
db.ogrenciler.find({ "notlar.mat": { $eq: 85 } })
```

### 🎯 4. Puanlar dizisi içinde tam olarak `[8, 9, 10]` olanlar (dizi karşılaştırması)

```js
db.ogrenciler.find({ puanlar: { $eq: [8, 9, 10] } })
```

> 🔥 Bu, sırası da dahil **aynı dizi** olmalı.

---

## 🛠 Not: `$eq` ile `alan: değer` farkı

```js
// Bu:
{ yas: 20 }

// Şu ile aynı:
{ yas: { $eq: 20 } }
```

Ama bu gibi durumlarda `yas` bir değişken ise:

```js
let deger = 20;
db.ogrenciler.find({ yas: { $eq: deger } });
```

gibi daha **dinamik** sorgular için `$eq` gereklidir.

---

## 🔍 Karşılaştırma: `$eq` vs `$in`

|Operatör|Kullanım|Örnek|
|---|---|---|
|`$eq`|Tam eşleşme|`{ yas: { $eq: 20 } }`|
|`$in`|Liste içinden eşleşme|`{ yas: { $in: [18, 20, 22] } }`|

---

## 🚫 `$eq` ile Hatalı Kullanım Örneği

```js
db.ogrenciler.find({ puanlar: { $eq: 9 } })
```

> ❌ Bu işe yaramaz çünkü `puanlar` bir **dizi**, `$eq: 9` doğrudan dizinin tamamı 9 olmalı gibi algılanır.  
> **Doğru:** `$in`, `$elemMatch`, `$gt` kullan.

---

