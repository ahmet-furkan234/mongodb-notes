
Belirtilen alanın, verilen değerden **küçük ya da ona eşit** olup olmadığını kontrol eder.

---

## 🔹 Temel Sözdizimi:

```js
{ alanAdi: { $lte: deger } }
```

---

## 🧪 Temel Örnekler

### 🎯 1. Yaşı 20 veya daha küçük olan öğrenciler:

```js
db.ogrenciler.find({ yas: { $lte: 20 } })
```

---

### 🎯 2. Ortalama puanı 75 veya altı olanlar:

```js
db.ogrenciler.find({ ortalama: { $lte: 75 } })
```

---

### 🎯 3. Matematik notu 85 ve altı olanlar:

```js
db.ogrenciler.find({ "notlar.mat": { $lte: 85 } })
```

---

### 🎯 4. `puanlar` dizisinde herhangi bir eleman 5 veya daha küçük olanlar:

```js
db.ogrenciler.find({ puanlar: { $lte: 5 } })
```

> ✅ `[3, 5, 6]` gibi diziler eşleşir  
> ❌ `[6, 7, 8]` eşleşmez

---

## 🧩 Birden Fazla `$lte` Koşulu

### 🎯 5. Yaşı en fazla 20 ve ortalaması en fazla 60 olanlar:

```js
db.ogrenciler.find({
  yas: { $lte: 20 },
  ortalama: { $lte: 60 }
})
```

---

## 📅 Tarihlerle `$lte`

```js
db.loglar.find({
  tarih: { $lte: new Date("2025-01-01") }
})
```

> 📦 Bu sorgu, 2025-01-01 tarihi **ve öncesine** ait kayıtları döner.

---

## 📌 `$lte` ile `$lt` Arasındaki Fark

|Operatör|Açıklama|20 değeri eşleşir mi?|
|---|---|---|
|`$lt`|Küçüktür|❌ Hayır|
|`$lte`|Küçük veya eşittir|✅ Evet|

---

## 🎯 Challenge Görevleri – `$lte`

1. Yaşı 18 ve altı olan öğrenciler
2. Ortalama puanı 70 ve altı olanlar
3. Matematik notu 60 ve altı olanlar
4. `puanlar` dizisinde 4 veya daha küçük not içerenler
5. Yaşı 20 veya daha küçük ve ortalaması 60 veya daha az olan öğrenciler
