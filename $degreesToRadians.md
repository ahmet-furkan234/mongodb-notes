
Derece (°) cinsinden verilen bir açıyı **radyan (rad)** cinsine çevirir.  
Bu, trigonometrik işlemler ya da **coğrafi koordinat** dönüşümleri için gereklidir.

---

### 📘 Söz Dizimi:

```js
{ $degreesToRadians: <number> }
```

- `number`: Dönüştürülecek derece değeri. Sabit sayı ya da alan referansı olabilir.

---

## 🔹 Temel Örnek:

```js
db.test.aggregate([
  {
    $project: {
      derece: 180,
      radyan: { $degreesToRadians: 180 }
    }
  }
])
```

**Sonuç:**

```json
{
  derece: 180,
  radyan: 3.141592653589793
}
```

---

## 🔸 Alan Üzerinde Örnek (Document Bazlı)

```js
db.LOKASYONLAR.aggregate([
  {
    $project: {
      sehir: 1,
      enlemDeg: "$LATITUDE",   // derece cinsinden
      enlemRad: { $degreesToRadians: "$LATITUDE" }
    }
  }
])
```

Bu dönüşüm sayesinde **coğrafi mesafe hesaplamalarında** doğru trigonometrik fonksiyonlar kullanılabilir.

---

## 🎯 Gerçek Hayat Örneği: İki Lokasyon Arası Mesafe Hesabı (Yarı çap = 6371 km)

İki nokta arasındaki mesafeyi Haversine formülü ile hesaplamak için radyan gereklidir.  
Aşağıda sadece `enlem` radyan dönüşümünü örnekliyoruz:

```js
db.LOKASYONLAR.aggregate([
  {
    $project: {
      city: "$CITY",
      latitude_deg: "$LATITUDE",
      longitude_deg: "$LONGITUDE",
      
      latitude_rad: { $degreesToRadians: "$LATITUDE" },
      longitude_rad: { $degreesToRadians: "$LONGITUDE" }
    }
  }
])
```

---

## 🧠 Ekstra Bilgi

|Derece|Radyan|
|---|---|
|0°|0|
|90°|π / 2|
|180°|π|
|270°|3π / 2|
|360°|2π|
