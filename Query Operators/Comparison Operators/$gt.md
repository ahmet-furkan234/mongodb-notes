
Belirtilen alanın, verilen değerden **büyük** olup olmadığını kontrol eder.

---

## 🔹 Temel Sözdizimi:

```js
{ alanAdi: { $gt: deger } }
```

---

## 🧪 Temel Örnekler:

### 🎯 1. Yaşı 20'den büyük öğrenciler:

```js
db.ogrenciler.find({ yas: { $gt: 20 } })
```

---

### 🎯 2. Ortalama puanı 75’ten büyük öğrenciler:

```js
db.ogrenciler.find({ ortalama: { $gt: 75 } })
```

---

### 🎯 3. Matematik notu 80’den büyük olanlar:

```js
db.ogrenciler.find({ "notlar.mat": { $gt: 80 } })
```

---

### 🎯 4. `puanlar` dizisinde herhangi bir elemanı 9’dan büyük olanlar:

```js
db.ogrenciler.find({ puanlar: { $gt: 9 } })
```

> 🔥 Bu, dizide herhangi bir eleman **9’dan büyükse eşleşir**  
> ✅ `[10, 9, 8]` gibi diziler eşleşir  
> ❌ `[7, 8, 9]` gibi diziler eşleşmez

---

## 🧩 `$gt` ile birden fazla koşul:

### 🎯 5. Yaşı 20’den büyük ve ortalaması 80’den büyük:

```js
db.ogrenciler.find({
  yas: { $gt: 20 },
  ortalama: { $gt: 80 }
})
```

---

## ⚠️ Dikkat Edilecek Noktalar:

|Dikkat|Açıklama|
|---|---|
|`$gt` sadece **sayısal veya tarih** karşılaştırmalarında mantıklıdır|String karşılaştırmalarda alfabetik sıralama olur|
|Dizilerde **herhangi bir eleman** büyükse eşleşir|`$elemMatch` ile daha kontrollü yapılabilir|
|İç içe alanlar için `"alan.alan"` sözdizimi kullanılır|`"notlar.mat"` gibi|

---

## 🧪 Challenge Görevleri – `$gt`

1. Yaşı 25’ten büyük öğrenciler
2. Ortalama puanı 85’ten büyük olanlar
3. Matematik notu 90 üstü olanlar
4. `puanlar` dizisinde 9’dan büyük puanı olanlar
5. Yaşı 20’den büyük ve ortalaması 75’ten fazla olanlar

---

## Ekstra – Tarih ile `$gt` Kullanımı

```js
db.loglar.find({
  tarih: { $gt: new Date("2025-01-01") }
})
```

> 📅 Bu, 2025 yılından sonra eklenmiş logları getirir.
