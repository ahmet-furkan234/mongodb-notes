
Bir diziden belirli sayıda elemanı veya belirli bir **aralıktaki** elemanları döndürür.  
Sadece **`find()` + projection** içinde kullanılır!

---

## 🔹 Temel Sözdizimi:

```js
db.koleksiyon.find(
  { filtre },
  { diziAlan: { $slice: sayı } }
)
```

> **Pozitif sayı:** dizinin başından itibaren o kadar eleman  
> **Negatif sayı:** dizinin sonundan itibaren o kadar eleman

---

## 🧪 Temel Örnekler

### 🎯 1. `puanlar` dizisinden ilk 2 elemanı getir:

```js
db.ogrenciler.find(
  {},
  { puanlar: { $slice: 2 }, _id: 0 }
)
```

---

### 🎯 2. `puanlar` dizisinden son 2 elemanı getir:

```js
db.ogrenciler.find(
  {},
  { puanlar: { $slice: -2 }, _id: 0 }
)
```

---

## 🔸 Belirli Aralıktaki Elemanlar

### 📌 Yapı:

```js
{ $slice: [başlangıçIndex, elemanSayısı] }
```

> Bu yapı `Array.slice(start, count)` gibi çalışır (pozitif index ile).

---

### 🎯 3. `puanlar` dizisinden 2. elemandan başlayarak 3 eleman al:

```js
db.ogrenciler.find(
  {},
  { puanlar: { $slice: [1, 3] }, _id: 0 }
)
```

---

## ⚠️ Notlar

- `$slice`, **filtre değil**, sadece gösterim (projection) içindir.
- **Sorgu kısmında** (`find({})`) kullanılamaz, sadece **projeksiyon kısmında** kullanılır.

---

## 🔀 Kombinasyon

### 🎯 4. Yaşı 20 olan öğrencilerden `puanlar` dizisinin son 1 elemanını getir:

```js
db.ogrenciler.find(
  { yas: 20 },
  { puanlar: { $slice: -1 }, _id: 0 }
)
```

---

## 🎯 Challenge Görevleri – `$slice`

1. `puanlar` dizisinden ilk 3 elemanı getir
2. `puanlar` dizisinin son 2 elemanını getir
3. 2. indexten itibaren 2 eleman getir
4. Yaşı 22 olanların `puanlar` dizisinin ilk 1 elemanını getir
5. `puanlar` dizisinden 3. elemandan itibaren 1 eleman getir