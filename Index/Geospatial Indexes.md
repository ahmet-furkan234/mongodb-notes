
MongoDB, **konum verisi** (coğrafi koordinatlar) ile çalışmayı sağlayan özel indeks türlerini destekler. Bu indeksler, aşağıdaki sorguları optimize eder:

- 📌 **Belirli bir konuma yakın yerleri bulmak**
- 📐 **Belirli bir bölge (dairsel, çokgensel) içinde yer alan kayıtları sorgulamak**
- 🚗 **Mesafeye göre sıralama yapmak**

---

## 🧭 Türleri

|Geospatial Index Türü|Açıklama|
|---|---|
|`2d` index|Düzlemsel (Cartesian koordinatlar), eski stil|
|`2dsphere` index|Küresel (gerçek dünya GPS verileri, WGS84 standardı)|
|`geoHaystack` (deprecated)|Büyük veri kümeleri için eskiden kullanılıyordu|

> Günümüzde en çok kullanılan ve önerilen: ✅ **`2dsphere`**

---

## 🌐 1. `2dsphere` Index – GPS Verisi için

### 🧠 Kullanım Alanı:

- Gerçek dünya koordinatları (latitude / longitude)
- WGS84 sistemine göre çalışır (Google Maps gibi)

---

### 🛠️ Örnek Veri:

```js
db.places.insertMany([
  {
    name: "Kahve Durağı",
    location: {
      type: "Point",
      coordinates: [28.9784, 41.0082] // [longitude, latitude]
    }
  },
  {
    name: "Kütüphane",
    location: {
      type: "Point",
      coordinates: [28.9768, 41.0100]
    }
  }
])
```

---

### 🧱 Index Oluşturma:

```js
db.places.createIndex({ location: "2dsphere" })
```

---

### 🔍 Konuma Göre Yakın Olanları Bulma:

```js
db.places.find({
  location: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [28.9770, 41.0090]
      },
      $maxDistance: 500  // metre cinsinden
    }
  }
})
```

Bu sorgu, **verilen konuma 500 metre mesafedeki yerleri en yakından uzağa sıralayarak döner.**

---

### 🔍 Belirli Bir Alan İçinde Olanları Bulma:

#### 🔘 Daire İçindekileri:

```js
db.places.find({
  location: {
    $geoWithin: {
      $centerSphere: [ [28.9770, 41.0090], 0.5 / 6378.1 ] // yarıçap km cinsinden / Dünya yarıçapı
    }
  }
})
```

#### 🔺 Polygon (Çokgen) İçindekiler:

```js
db.places.find({
  location: {
    $geoWithin: {
      $geometry: {
        type: "Polygon",
        coordinates: [[
          [28.975, 41.007],
          [28.980, 41.007],
          [28.980, 41.011],
          [28.975, 41.011],
          [28.975, 41.007]
        ]]
      }
    }
  }
})
```

---

## 🚧 Dikkat Edilmesi Gerekenler

|Konu|Detay|
|---|---|
|Koordinat sırası|`[longitude, latitude]` (X, Y) — karışmamalı|
|Tip alanı|`"type": "Point"` mutlaka belirtilmeli|
|2dsphere ile uyumlu|Sorgular ve veriler GeoJSON standardına uymalı|
|2d index|Sadece 2 boyutlu düzlem verisi içindir, GPS için uygun değil|
|Index zorunlu|Geospatial sorgular **indexsiz çalışmaz ya da çok yavaştır**|

---

## ✅ Uygulama Alanları

- Google Maps benzeri lokasyon sorgulamaları
- E-ticaret kargo dağıtım yakınlık hesaplamaları
- En yakın mağaza, şube, ATM, vs.
- Navigasyon sistemleri
- IoT cihaz takibi (araçlar, sensörler, vs.)

---

## Özet

|Özellik|Açıklama|
|---|---|
|En çok kullanılan|`2dsphere`|
|Veri tipi|`GeoJSON` (Point, Polygon, vb.)|
|Sorgular|`$near`, `$geoWithin`, `$geoIntersects`|
|Performans|Coğrafi index olmadan çalışmaz veya çok yavaş olur|
|Uygulama alanı|Harita, navigasyon, yakınlık, bölge içi arama|
