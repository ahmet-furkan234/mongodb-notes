
## 📁 Örnek Veri Seti: `orders`

```js
db.orders.insertMany([
  { customer: "Ali",  product: "TV",    total: 1000 },
  { customer: "Ali",  product: "Phone", total: 500 },
  { customer: "Ali",  product: "TV",    total: 1200 },
  { customer: "Ayşe", product: "Tablet",total: 800 },
  { customer: "Ayşe", product: "Phone", total: 600 }
])
```

---

## 1. 🧮 `$sum` — Toplama

### 📌 Her müşterinin toplam harcaması:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      totalSpent: { $sum: "$total" }
    }
  }
])
```

> ✅ Ali → 2700, Ayşe → 1400

---

### 📌 Her ürünün sipariş sayısı:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$product",
      count: { $sum: 1 }
    }
  }
])
```

> ✅ TV: 2, Phone: 2, Tablet: 1

---

## 2. ➗ `$avg` — Ortalama

### 📌 Her müşterinin ortalama sipariş tutarı:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      avgOrderValue: { $avg: "$total" }
    }
  }
])
```

> ✅ Ali → 900, Ayşe → 700

---

### 📌 Ürün bazında ortalama fiyat:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$product",
      avgPrice: { $avg: "$total" }
    }
  }
])
```

> ✅ TV: 1100, Phone: 550, Tablet: 800

---

## 3. 📉 `$min` — Minimum Değer

### 📌 Her müşterinin en düşük harcaması:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      minTotal: { $min: "$total" }
    }
  }
])
```

> ✅ Ali → 500, Ayşe → 600

---

### 📌 Ürün bazında en düşük fiyat:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$product",
      lowestPrice: { $min: "$total" }
    }
  }
])
```

> ✅ TV → 1000, Phone → 500, Tablet → 800

---

## 4. 📈 `$max` — Maksimum Değer

### 📌 Her müşterinin en büyük siparişi:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      maxTotal: { $max: "$total" }
    }
  }
])
```

> ✅ Ali → 1200, Ayşe → 800

---

### 📌 Ürün bazında en yüksek fiyat:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$product",
      highestPrice: { $max: "$total" }
    }
  }
])
```

> ✅ TV: 1200, Phone: 600, Tablet: 800

---

## 🔄 Tümünü birlikte kullanmak:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      total: { $sum: "$total" },
      average: { $avg: "$total" },
      min: { $min: "$total" },
      max: { $max: "$total" }
    }
  }
])
```

> ✅ Bir arada harcama analizi çıkarır.

---

## 🧠 Teknik Notlar

|Operatör|Açıklama|Sayısal Alan Gerekli?|
|---|---|---|
|`$sum`|Toplam değer veya belge sayısı|✅ (ya da `$sum: 1`)|
|`$avg`|Ortalama değer|✅|
|`$min`|En küçük değer|✅|
|`$max`|En büyük değer|✅|

---

## 🧮 Ekstra: `$sum: 1` ne işe yarar?

Belge saymak için kullanılır:

```js
$group: {
  _id: "$product",
  count: { $sum: 1 }
}
```

Bu ifade, her belge için 1 ekleyerek sayma işlemi yapar. Tıpkı SQL'deki `COUNT(*)` gibi.
