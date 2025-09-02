
- Vergi hesaplamaları
- İndirim, oran, yüzdelik sonuçları
- Grafiklerde küsurat temizleme
- Sayısal sonuçları estetikleştirme
gibi işlemler için sıkça kullanılır.

```js
{ $round: [ <değer>, <basamak sayısı> ] }
```

- `<değer>`: Yuvarlanacak sayı veya alan    
- `<basamak sayısı>` (opsiyonel): Virgülden sonra kaç basamak olsun.
    - Varsayılan: `0` (yani tam sayıya yuvarlar) 

---

## 📁 Örnek Veri: `products`

```js
db.products.insertMany([
  { name: "TV",     price: 999.789 },
  { name: "Phone",  price: 1250.146 },
  { name: "Laptop", price: 3099.5 }
])
```

---

## 📌 1. Sayıyı tam sayıya yuvarla (default davranış)

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      roundedPrice: { $round: ["$price", 0] }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", roundedPrice: 1000 }
{ name: "Phone", roundedPrice: 1250 }
{ name: "Laptop", roundedPrice: 3100 }
```

---

## 📌 2. Virgülden sonra 2 basamağa yuvarla

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      roundedTo2Decimals: { $round: ["$price", 2] }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", roundedTo2Decimals: 999.79 }
{ name: "Phone", roundedTo2Decimals: 1250.15 }
```

---

## 📌 3. Hesap sonrası sonucu yuvarlama (indirimli fiyat)

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      discountPrice: {
        $round: [
          { $multiply: ["$price", 0.9] }, // %10 indirim
          2
        ]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", discountPrice: 899.81 }
{ name: "Phone", discountPrice: 1125.13 }
```

---

## 📌 4. `$divide` + `$round`: Yüzde hesaplama

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      percentOfMax: {
        $round: [
          { $multiply: [{ $divide: ["$price", 3100] }, 100] },
          1
        ]
      }
    }
  }
])
```

---

## 📌 5. Negatif basamak sayısı: **10'luk basamaklara yuvarlama**

> Negatif basamaklar, **tam sayıdan önceki basamaklara** yuvarlar:

```js
{ $round: [ 1234.56, -2 ] } // → 1200
```

---

## 📌 6. `$cond` ile birlikte kullanımı

```js
{
  $project: {
    name: 1,
    specialPrice: {
      $round: [
        {
          $cond: {
            if: { $gt: ["$price", 1000] },
            then: { $multiply: ["$price", 0.95] }, // %5 indirim
            else: "$price"
          }
        },
        2
      ]
    }
  }
}
```

---

## ⚠️ Dikkat Edilecekler

|Durum|Etki|
|---|---|
|Basamak sayısı 0|Tam sayıya yuvarlar|
|Negatif basamak|10’luk katlara yuvarlar|
|`null` değeri|Sonuç `null` olur|
|String değeri|❌ Hata verir|

---

## ✅ Özet

|Özellik|Destekleniyor|
|---|---|
|Sayıyı tam sayıya yuvarlama|✅|
|x basamaklı ondalıklı yuvarlama|✅|
|İşlem sonucu yuvarlama|✅|
|Negatif basamakla yuvarlama|✅|
|`$cond`, `$multiply`, `$divide` ile kombinasyon|✅|

---

### 🔁 Alternatif Yuvarlama Operatörleri

|Operatör|Açıklama|
|---|---|
|`$round`|Belirli basamağa yuvarlar|
|`$floor`|Aşağı yuvarlar|
|`$ceil`|Yukarı yuvarlar|
|`$trunc`|Ondalık kısmı atar (kırpar)|
