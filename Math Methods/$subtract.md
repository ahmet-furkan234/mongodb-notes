
> İki değeri birbirinden çıkarır.  
> Hem **sayılar** hem **tarihler (ISODate)** ile çalışabilir.

📌 Söz dizimi:

```js
{ $subtract: [ <değer1>, <değer2> ] }
```

> `değer1 - değer2`

---

## 📁 Örnek Veri: `products`

```js
db.products.insertMany([
  { name: "TV",     price: 1000, discount: 200 },
  { name: "Phone",  price: 2000, discount: 250 },
  { name: "Laptop", price: 3000, discount: 0 }
])
```

---

## 📌 1. price - discount → net fiyat

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      netPrice: {
        $subtract: ["$price", "$discount"]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", netPrice: 800 }
{ name: "Phone", netPrice: 1750 }
{ name: "Laptop", netPrice: 3000 }
```

---

## 📌 2. Tarihler Arası Fark (ms cinsinden)

### Örnek veri:

```js
db.sessions.insertOne({
  user: "TkMatE",
  loginTime: ISODate("2025-07-13T08:00:00Z"),
  logoutTime: ISODate("2025-07-13T10:30:00Z")
})
```

```js
db.sessions.aggregate([
  {
    $project: {
      user: 1,
      durationInMs: {
        $subtract: ["$logoutTime", "$loginTime"]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ user: "TkMatE", durationInMs: 9000000 } // 2.5 saat → ms
```

---

### 🧮 Milisaniyeyi saate çevirme

```js
{
  $divide: [
    { $subtract: ["$logoutTime", "$loginTime"] },
    1000 * 60 * 60
  ]
}
```

> ✅ Böylece kaç saat sürdüğünü sayısal olarak elde edersin.

---

## 📌 3. `$subtract` + `$cond` ile kontrol

> Eğer indirim sıfır değilse çıkart, yoksa olduğu gibi bırak:

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      netPrice: {
        $cond: {
          if: { $gt: ["$discount", 0] },
          then: { $subtract: ["$price", "$discount"] },
          else: "$price"
        }
      }
    }
  }
])
```

---

## 📌 4. Nested `$subtract` ile: `fiyat - indirim - kupon`

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      finalPrice: {
        $subtract: [
          { $subtract: ["$price", "$discount"] },
          100 // kupon indirimi
        ]
      }
    }
  }
])
```

---

## 📌 5. Günlük veya saatlik fark hesaplama (tarih karşılaştırmalı)

```js
db.logs.aggregate([
  {
    $project: {
      createdAt: 1,
      now: "$$NOW",
      diffInHours: {
        $divide: [
          { $subtract: ["$$NOW", "$createdAt"] },
          1000 * 60 * 60
        ]
      }
    }
  }
])
```

> Bu örnek sayesinde belge ne kadar önce oluşturulmuş, saat cinsinden ölçülür.

---

## ⚠️ Dikkat Edilecekler

|Durum|Açıklama|
|---|---|
|`null` değer varsa|Sonuç `null` olur|
|Sıralama yanlışsa|`$subtract: [5, 10]` → `-5`|
|Tarih çıkarımı|Sonuç `ms` cinsindendir|
|Daha fazla değer|❌ Sadece **iki operand** alır|

---

## ✅ Özet

|Özellik|Destekleniyor|
|---|---|
|Sayıdan sayı çıkarma|✅|
|Alan – alan|✅|
|Alan – sabit|✅|
|Tarih – tarih (fark)|✅ (ms)|
|Nested `$subtract`|✅|
|Koşullu çıkarma (`$cond`)|✅|

---

Hazırsan şimdi:

- `$mod` (kalan bulma),
    
- `$round`, `$floor`, `$ceil` (yuvarlama),
    
- `$cond` (koşul),
    
- `$abs` (mutlak değer),
    
- `$trunc` (ondalık kırpma)
    

gibi matematiksel operatörlerle veya `$dateAdd`, `$dateSubtract`, `$dateDiff` gibi zaman işlemleriyle devam edebiliriz.

Hangisiyle ilerleyelim TkMatE?