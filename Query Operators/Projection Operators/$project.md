
### 📌 Görevi:

- Hangi alanların dahil edileceğini/gizleneceğini belirler
- Yeni alanlar oluşturur (hesaplamalı, formatlı vs.)
- Alan adlarını değiştirir
- Dizileri işler, içinden slice alır
- Koşullu (ternary gibi) dönüşüm yapar

> 🔥 Yani `select`, `as`, `case when`, `substring`, `slice`, `alias`, `computed field` gibi SQL işlerini yapar.

---

## 🔹 Temel Kullanım

```js
db.koleksiyon.aggregate([
  {
    $project: {
      alan1: 1,
      alan2: 0,
      yeniAlan: "değer",
      başkaAlan: { işlem }
    }
  }
])
```

---

## 🧪 Temel Örnekler

### 🎯 1. Sadece `ad`, `yas` ve `ortalama` alanlarını göster:

```js
db.ogrenciler.aggregate([
  {
    $project: {
      ad: 1,
      yas: 1,
      ortalama: 1,
      _id: 0
    }
  }
])
```

---

### 🎯 2. Yeni bir alan oluştur: `aktif` değeri her zaman `true` olsun

```js
db.ogrenciler.aggregate([
  {
    $project: {
      ad: 1,
      aktif: true,
      _id: 0
    }
  }
])
```

---

### 🎯 3. `yas` ile `ortalama`'yı toplayıp `puanToplam` alanı oluştur:

```js
db.ogrenciler.aggregate([
  {
    $project: {
      ad: 1,
      puanToplam: { $add: ["$yas", "$ortalama"] },
      _id: 0
    }
  }
])
```

---

### 🎯 4. `puanlar` dizisinden sadece ilk 2 eleman:

```js
db.ogrenciler.aggregate([
  {
    $project: {
      ad: 1,
      ilkPuanlar: { $slice: ["$puanlar", 2] },
      _id: 0
    }
  }
])
```

---

### 🎯 5. Hem baştan hem sondan diziden eleman çek (normal `find` ile yapılamaz!)

```js
db.ogrenciler.aggregate([
  {
    $project: {
      ad: 1,
      ilk2: { $slice: ["$puanlar", 2] },
      son1: { $slice: ["$puanlar", -1] },
      _id: 0
    }
  }
])
```

---

## 💡 Ekstra: Alan Adını Değiştir (alias gibi)

```js
db.ogrenciler.aggregate([
  {
    $project: {
      isim: "$ad",   // "ad" alanını "isim" adıyla göster
      _id: 0
    }
  }
])
```

---

## 🎯 Challenge Görevleri – `$project`

1. Sadece `ad`, `yas`, `ortalama` göster
2. `yas` + `ortalama` toplamını `toplam` alanı olarak göster
3. `puanlar` dizisinin sadece son 1 elemanını göster
4. `puanlar` dizisinin ilk 2 ve son 1 elemanını **farklı adlarla** göster
5. `ortalama` 80 ve üzeriyse `durum: "Geçti"` değilse `"Kaldı"` (Koşullu proje)
