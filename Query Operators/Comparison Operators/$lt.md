
Belirtilen alanın, verilen değerden **küçük** olup olmadığını kontrol eder.

---

## 🔹 Temel Sözdizimi:

```js
{ alanAdi: { $lt: deger } }
```

---

## 🧪 Temel Örnekler

### 🎯 1. Yaşı 20’den küçük öğrenciler:

```js
db.ogrenciler.find({ yas: { $lt: 20 } })
```

---

### 🎯 2. Ortalama puanı 70’ten düşük olan öğrenciler:

```js
db.ogrenciler.find({ ortalama: { $lt: 70 } })
```

---

### 🎯 3. Matematik notu 50’den az olanlar:

```js
db.ogrenciler.find({ "notlar.mat": { $lt: 50 } })
```

---

### 🎯 4. `puanlar` dizisinde herhangi bir eleman 5’ten küçük olanlar:

```js
db.ogrenciler.find({ puanlar: { $lt: 5 } })
```

> ✅ `[3, 5, 6]` gibi diziler eşleşir  
> ❌ `[5, 6, 7]` gibi diziler eşleşmez

---

## 🧩 Birden Fazla `$lt` Koşulu

### 🎯 5. Yaşı 20'den küçük ve ortalaması 65’ten düşük öğrenciler:

```js
db.ogrenciler.find({
  yas: { $lt: 20 },
  ortalama: { $lt: 65 }
})
```

---

## 🧠 `$lt` ile Dizi Dikkati

```js
db.ogrenciler.find({ puanlar: { $lt: 7 } })
```

Bu sorgu, **puanlar dizisinde 7’den küçük herhangi bir sayı varsa eşleşir.**

> Yani `[5, 6, 7]` ✅  
> `[8, 9, 10]` ❌

---

## 📅 Tarihlerle `$lt`

```js
db.loglar.find({
  tarih: { $lt: new Date("2025-01-01") }
})
```

> Bu, 2025'ten **önceki** logları getirir.

---

## 🎯 Challenge Görevleri – `$lt`

1. Yaşı 18’den küçük öğrenciler
2. Ortalama puanı 60’tan az olanlar
3. Matematik notu 45’ten düşük olanlar
4. `puanlar` dizisinde 4’ten küçük not olanlar
5. Yaşı 20’den küçük ve ortalaması 65’ten düşük olanlar
