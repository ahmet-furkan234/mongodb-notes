
> `$trunc`, bir sayının **ondalık kısmını siler** (kırpar).  
> **Yuvarlama yapmaz** → sadece keser.

📌 Söz dizimi:

```js
{ $trunc: <değer> }
// veya
{ $trunc: [ <değer>, <ondalık basamak sayısı> ] }
```

---

## 🧪 Kısa Karşılaştırma

|Girdi|`$floor`|`$ceil`|`$round`|`$trunc`|
|---|---|---|---|---|
|99.8|99|100|100|99|
|-99.8|-100|-99|-100|-99|

---

## 📁 Örnek Veri: `orders`

```js
db.orders.insertMany([
  { customer: "Ali", total: 1234.567 },
  { customer: "Ayşe", total: 999.999 },
  { customer: "Veli", total: 107.123456 }
])
```

---

## 📌 1. Tam sayıya kırpma (ondalıkları sil)

```js
db.orders.aggregate([
  {
    $project: {
      customer: 1,
      totalTruncated: { $trunc: "$total" }
    }
  }
])
```

📤 Çıktı:

```json
{ customer: "Ali", totalTruncated: 1234 }
{ customer: "Ayşe", totalTruncated: 999 }
{ customer: "Veli", totalTruncated: 107 }
```

---

## 📌 2. Belirli basamağa kırpma (ondalık kısmı tutarak)

```js
db.orders.aggregate([
  {
    $project: {
      customer: 1,
      truncatedTo2Decimals: {
        $trunc: ["$total", 2]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ customer: "Ali", truncatedTo2Decimals: 1234.56 }
{ customer: "Ayşe", truncatedTo2Decimals: 999.99 }
{ customer: "Veli", truncatedTo2Decimals: 107.12 }
```

---

## 📌 3. Negatif basamak sayısı

> Negatif değer verirsen tam sayıdan **önceki** basamaklar kırpılır.

```js
db.orders.aggregate([
  {
    $project: {
      customer: 1,
      truncToHundred: {
        $trunc: ["$total", -2]
      }
    }
  }
])
```

📤 Örnek:

```json
{ customer: "Ali", truncToHundred: 1200 }
{ customer: "Ayşe", truncToHundred: 900 }
```

---

## 📌 4. Hesap sonucu kırpma

```js
db.orders.aggregate([
  {
    $project: {
      customer: 1,
      netAmount: {
        $trunc: [
          { $multiply: ["$total", 1.08] }, // %8 KDV
          2
        ]
      }
    }
  }
])
```

---

## 📌 5. `$cond` ile birlikte

> Eğer fiyat 1000’den büyükse 1 basamak, değilse tam sayıya kırp:

```js
db.orders.aggregate([
  {
    $project: {
      customer: 1,
      smartTrunc: {
        $trunc: [
          "$total",
          {
            $cond: {
              if: { $gt: ["$total", 1000] },
              then: 1,
              else: 0
            }
          }
        ]
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

|Girdi|Sonuç|
|---|---|
|123.456|123|
|-123.456|-123|
|null|null|
|string|❌ Hata|

---

## ✅ Özet

|Özellik|Destekleniyor|
|---|---|
|Ondalık kısmı kırpma|✅|
|Belirli basamağa kırpma|✅|
|Negatif basamakla yüzlük, binlik|✅|
|Hesap sonrası trunc|✅|
|`$cond` ile koşullu trunc|✅|
|Yuvarlama yok! (sadece keser)|✅|

---

## 📚 Karşılaştırmalı Örnek

|Operatör|Girdi|Sonuç|
|---|---|---|
|`$round`|123.567|124|
|`$floor`|123.567|123|
|`$ceil`|123.001|124|
|`$trunc`|123.567|123|

---

Hazırsan sırada:

- ➕ `$abs` (mutlak değer),
    
- 🎯 `$cond` (koşullu ifade),
    
- 🔁 `$switch`,
    
- 📆 `$dateAdd`, `$dateSubtract`, `$dateDiff` gibi zaman işlemleri
    

Hangisiyle devam edelim TkMatE?