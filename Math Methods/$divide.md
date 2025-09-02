
> İki sayıyı birbirine **böler**.  
> Söz dizimi:

```js
{ $divide: [ <bölen>, <bölünen> ] }
```

> ➕ `Math.floor`, `round`, `toFixed` gibi dönüşümler **ekstra yapılır**, doğrudan `toFixed` gibi şeyler içermez.

---

## ✅ Temel Kullanım

```js
{ $divide: [ "$fiyat", 2 ] }
```

- `$fiyat` alanını 2’ye böler.
- Sonuç **float** (ondalıklı sayı) olur.

---

## 📁 Örnek Veri: `products`

```js
db.products.insertMany([
  { name: "TV",     price: 1000, discount: 100 },
  { name: "Phone",  price: 2000, discount: 400 },
  { name: "Laptop", price: 3000, discount: 0 }
])
```

---

## 📌 1. İndirim oranını hesapla (discount / price)

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      price: 1,
      discountRate: {
        $divide: ["$discount", "$price"]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV",     price: 1000, discountRate: 0.1 }
{ name: "Phone",  price: 2000, discountRate: 0.2 }
{ name: "Laptop", price: 3000, discountRate: 0 }
```

---

## 📌 2. `%` cinsinden indirim oranı hesapla

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      discountPercentage: {
        $multiply: [
          { $divide: ["$discount", "$price"] },
          100
        ]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", discountPercentage: 10 }
{ name: "Phone", discountPercentage: 20 }
```

---

## 📌 3. Bölüm sonucu tam sayı yapmak için `$round`

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      roundedRate: {
        $round: [
          { $multiply: [ { $divide: ["$discount", "$price"] }, 100 ] },
          0
        ]
      }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", roundedRate: 10 }
{ name: "Phone", roundedRate: 20 }
```

---

## ⚠️ Bölünen sıfır olursa?

```js
{ $divide: [ "$price", "$discount" ] }
```

Eğer `$discount` sıfırsa veya null ise, sonuç **hata vermez** ama `null` döner.

> ✅ En iyi pratik: `$cond` ile kontrol:

```js
{
  $project: {
    name: 1,
    safeDivision: {
      $cond: {
        if: { $eq: ["$discount", 0] },
        then: "NA",
        else: { $divide: ["$price", "$discount"] }
      }
    }
  }
}
```

---

## 💡 Uygulama Fikri: KDV Dahil Fiyat Hesabı

> KDV %18, yeni fiyat = fiyat + (fiyat * 0.18)

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      price: 1,
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

---

## ✅ Özet

|Özellik|Destekleniyor|
|---|---|
|Alan / sayı bölme|✅|
|Alan / alan bölme|✅|
|Sonucu yüzdeye çevirme|✅ (çarpı 100)|
|Bölümde sıfır kontrolü|✅ `$cond` ile|
|`round`, `floor`, vs.|✅ `$round`, `$trunc`|

---

Hazırsan şimdi `$multiply`, `$subtract`, `$round`, `$cond`, `$mod`, `$ceil`, `$floor` gibi diğer matematiksel operatörlerle devam edebiliriz.

Hangisini detaylı alalım TkMatE?