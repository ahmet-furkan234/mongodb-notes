
Belirtilen alanın, verilen değerden **büyük veya eşit** olup olmadığını kontrol eder.

---

## 🔹 Temel Sözdizimi:

```js
{ alanAdi: { $gte: deger } }
```

---

## 🧪 Temel Örnekler

### 🎯 1. Yaşı 20 veya daha büyük olan öğrenciler:

```js
db.ogrenciler.find({ yas: { $gte: 20 } })
```

---

### 🎯 2. Ortalama puanı 80 ve üstü olanlar:

```js
db.ogrenciler.find({ ortalama: { $gte: 80 } })
```

---

### 🎯 3. Matematik notu en az 90 olanlar:

```js
db.ogrenciler.find({ "notlar.mat": { $gte: 90 } })
```

---

### 🎯 4. `puanlar` dizisinde herhangi bir eleman 9 veya daha büyük olanlar:

```js
db.ogrenciler.find({ puanlar: { $gte: 9 } })
```

> ✅ `[8, 9, 10]` ve `[9, 10, 9]` gibi diziler eşleşir  
> ❌ `[5, 6, 8]` gibi diziler eşleşmez

---

## 🧩 Birden Fazla `$gte` Koşulu

### 🎯 5. Yaşı 18 ve üstü, ortalaması 70 ve üstü:

```js
db.ogrenciler.find({
  yas: { $gte: 18 },
  ortalama: { $gte: 70 }
})
```

---

## 📅 Tarihlerle `$gte`

```js
db.loglar.find({
  tarih: { $gte: new Date("2025-01-01") }
})
```

> 📦 Bu sorgu, 2025-01-01 **ve sonrasındaki** tarihleri getirir.

---

## 🧠 `$gte` ile `$gt` Arasındaki Fark

|Operatör|Açıklama|20 değeri eşleşir mi?|
|---|---|---|
|`$gt`|Büyüktür|❌ Hayır|
|`$gte`|Büyük veya eşittir|✅ Evet|

---

## 🎯 Challenge Görevleri – `$gte`

1. Yaşı 22 ve üzeri olan öğrenciler
2. Ortalama puanı 85 ve üzeri olanlar
3. Matematik notu en az 95 olanlar
4. `puanlar` dizisinde 10 ve üzeri not içerenler
5. Yaşı 20 ve üzeri ve ortalaması 80 veya daha fazla olanlar
