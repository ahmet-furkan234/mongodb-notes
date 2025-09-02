
> `$add`, iki veya daha fazla değeri **toplar**.  
> Sayısal veya **tarih türü** verilerle çalışabilir.

📌 Söz dizimi:

```js
{ $add: [ <değer1>, <değer2>, ... ] }
```

---

## 📁 Örnek Veri: `products`

```js
db.products.insertMany([
  { name: "TV", price: 1000, tax: 180 },
  { name: "Phone", price: 2000, tax: 360 },
  { name: "Laptop", price: 3000, tax: 540 }
])
```

---

## 📌 1. price + tax ⇒ toplam fiyat

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      totalPrice: { $add: ["$price", "$tax"] }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", totalPrice: 1180 }
{ name: "Phone", totalPrice: 2360 }
{ name: "Laptop", totalPrice: 3540 }
```

---

## 📌 2. price + (price × %18) ⇒ KDV dahil fiyat

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      priceWithTax: {
        $add: [
          "$price",
          { $multiply: ["$price", 0.18] }
        ]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", priceWithTax: 1180 }
```

---

## 📌 3. 3 veya daha fazla sayı toplama

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      grandTotal: {
        $add: ["$price", "$tax", 100]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", grandTotal: 1280 }
```

---

## 📌 4. Tarihe süre ekleme (datetime + milliseconds)

### Örnek Veri: `events`

```js
db.events.insertOne({
  name: "Webinar",
  startTime: ISODate("2025-07-13T10:00:00Z")
})
```

> 🔎 1 saat (3600000 ms) sonrasını hesapla:

```js
db.events.aggregate([
  {
    $project: {
      name: 1,
      endTime: {
        $add: [
          "$startTime",
          1000 * 60 * 60 // 1 saat
        ]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "Webinar", endTime: ISODate("2025-07-13T11:00:00Z") }
```

---

## 📌 5. `$cond` ile koşullu toplama

> Eğer fiyat 1500’den fazlaysa +200, değilse +50 ekle:

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      adjustedPrice: {
        $add: [
          "$price",
          {
            $cond: {
              if: { $gt: ["$price", 1500] },
              then: 200,
              else: 50
            }
          }
        ]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", adjustedPrice: 1050 }
{ name: "Phone", adjustedPrice: 2200 }
```

---

## ✅ Uyumlu Veri Tipleri

|Tip|Açıklama|
|---|---|
|Sayılar|✅ Doğrudan toplanabilir|
|Tarihler|✅ Millisaniye eklenebilir|
|`null`|❌ Hata vermez, `null` kabul edilir (toplam etkilenir)|

---

## ⚠️ Dikkat Edilecekler

|Durum|Etki|
|---|---|
|Null + sayı|Sonuç `null` olur|
|String + sayı|❌ Hata verir|
|Çok fazla operand|✅ Sorun değil|
|Zaman hesaplamasında timezone farkı|UTC döner|

---

## ✅ Özet

|Özellik|Kullanımı|
|---|---|
|Sayı + sayı toplama|✅|
|Alan + sabit değer|✅|
|Alan + alan|✅|
|Tarihe süre ekleme|✅|
|`%` hesaplama + toplama|✅|
|Koşullu toplama (`$cond`)|✅|

---

Hazırsan şimdi istersen:

- `$subtract` (çıkarma)
- `$mod` (mod alma)
    
- `$round` / `$floor` / `$ceil` (yuvarlama işlemleri)
    
- `$cond` (if-else)
    
- `$dateAdd` / `$dateSubtract` (tarih işlemleri)
    

gibi operatörlerle devam edebilirim.  
Hangisiyle ilerleyelim TkMatE?