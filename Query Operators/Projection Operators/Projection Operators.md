
MongoDB’de `find()` sorgusu, varsayılan olarak her belgenin **tüm alanlarını** getirir.  
**Projection** sayesinde sadece istediğin alanları seçebilirsin.

---

## 🔹 Temel Kullanım:

```js
db.koleksiyon.find(
  { filtre },         // → arama koşulları
  { projection }      // → hangi alanlar gösterilecek/gösterilmeyecek
)
```

---

## 📌 1. Alanları Gösterme (`1` ile işaretleme)

### 🎯 Sadece `ad` ve `yas` alanlarını getir:

```js
db.ogrenciler.find(
  {},
  { ad: 1, yas: 1 }
)
```

> 🔹 `_id` alanı varsayılan olarak her zaman gelir.  
> 🔹 Onu gizlemek istersen ayrıca belirtmelisin:

```js
db.ogrenciler.find(
  {},
  { ad: 1, yas: 1, _id: 0 }
)
```

---

## 📌 2. Alanları Gizleme (`0` ile işaretleme)

### 🎯 `ortalama` ve `yas` harici tüm alanları getir:

```js
db.ogrenciler.find(
  {},
  { ortalama: 0, yas: 0 }
)
```

> ❗ `1` ve `0` projeksiyonları birlikte **kullanılamaz**, sadece `_id` istisnadır.

---

## 📌 3. Dizi Alt Alanları (Array Slicing)

### 🎯 `puanlar` dizisinden ilk 2 elemanı getir:

```js
db.ogrenciler.find(
  {},
  { puanlar: { $slice: 2 } }
)
```

### 🎯 `puanlar` dizisinden 3. elemandan itibaren 2 tane:

```js
db.ogrenciler.find(
  {},
  { puanlar: { $slice: [2, 2] } }
)
```

---

## 📌 4. Belge Alt Alanları

### 🎯 Sadece `notlar.mat` alanını getir:

```js
db.ogrenciler.find(
  {},
  { "notlar.mat": 1, _id: 0 }
)
```

> Diğer tüm alt alanlar otomatik olarak gizlenir.

---

## 📌 5. `$elemMatch` ile dizide eşleşen TEK bir elemanı getir

### 🎯 `puanlar` dizisinde sadece 9'dan büyük ilk elemanı getir:

```js
db.ogrenciler.find(
  {},
  { puanlar: { $elemMatch: { $gt: 9 } }, _id: 0 }
)
```

---

## 🔄 En Sık Kullanım Kombinasyonları

|Amaç|Projeksiyon|
|---|---|
|Sadece bazı alanları getir|`{ alan1: 1, alan2: 1, _id: 0 }`|
|Bazı alanları hariç tut|`{ alan1: 0, alan2: 0 }`|
|Belirli dizi elemanlarını getir|`{ puanlar: { $slice: 3 } }`|
|Belirli koşulla dizi elemanı getir|`{ puanlar: { $elemMatch: { $lt: 8 } } }`|

---

## 🎯 Challenge Görevleri – Projection

1. Sadece `ad`, `yas`, `ortalama` alanlarını getir (`_id` hariç)
    
2. `notlar` alanını tamamen hariç tut
    
3. `puanlar` dizisinin ilk 2 elemanını getir
    
4. `puanlar` dizisinde sadece 10’dan büyük ilk elemanı getir
    
5. Sadece `notlar.mat` ve `notlar.fizik` alanlarını getir
    

---

Hazırsan bu challenge’ları birlikte yazabiliriz  
veya bir sonraki konu olan **Array Operators** grubuna geçebiliriz (`$in`, `$all`, `$elemMatch`, `$size`, vs.)  
Hangisi olsun TkMatE?