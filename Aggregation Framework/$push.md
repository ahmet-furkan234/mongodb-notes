
> `$push`, her bir **grup** için belirtilen alanın tüm değerlerini bir **dizi içinde toplar**.
- Aynı gruba ait tüm değerleri sırayla bir araya getirir.
- Diziye **tekrarlı** olarak ekler (benzersiz filtrelemesi yoktur).
- Eğer **benzersiz değerler** istiyorsan `$addToSet` kullanılır.

---

## 📁 Örnek Veri: `orders`

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

## 📌 1. Her müşterinin aldığı ürünleri listele

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      products: { $push: "$product" }
    }
  }
])
```

> **Çıktı:**

```json
[
  { _id: "Ali", products: ["TV", "Phone", "TV"] },
  { _id: "Ayşe", products: ["Tablet", "Phone"] }
]
```

---

## 📌 2. `$push` ile obje olarak değer toplama

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      orders: {
        $push: {
          item: "$product",
          amount: "$total"
        }
      }
    }
  }
])
```

> ✅ Her müşterinin tüm sipariş bilgileri `{item, amount}` formatında diziye alınır.

**Çıktı (örnek):**

```json
{
  _id: "Ali",
  orders: [
    { item: "TV", amount: 1000 },
    { item: "Phone", amount: 500 },
    { item: "TV", amount: 1200 }
  ]
}
```

---

## 📌 3. `$match` + `$group` + `$push` kombinasyonu

> Sadece 1000 TL üzeri siparişleri toplayıp müşteri bazında ürünleri listele:

```js
db.orders.aggregate([
  { $match: { total: { $gt: 1000 } } },
  {
    $group: {
      _id: "$customer",
      expensiveItems: { $push: "$product" }
    }
  }
])
```

> ✅ 1000 TL üstü ürünleri müşteri bazlı listelemiş olduk.

---

## 📌 4. `$push` yerine `$addToSet` kullanırsan?

```js
$addToSet: "$product"
```

> Aynı müşteri aynı ürünü 10 kez alsa bile **bir kez** görünür.

---

## 💡 Karşılaştırma

|Operatör|Açıklama|
|---|---|
|`$push`|Tüm değerleri sırayla ekler (kopya olabilir)|
|`$addToSet`|Sadece benzersiz değerleri ekler|

---

## 🎯 Gerçek Hayat Senaryosu

> "Her müşterinin verdiği siparişlerin detaylarını sırayla tut"

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      history: {
        $push: {
          product: "$product",
          total: "$total"
        }
      }
    }
  }
])
```

---

## 🧠 Teknik Not

- `$push` içinde `$cond`, `$substr`, `$toUpper`, `$multiply` gibi tüm operatörleri kullanabilirsin.
    
- Dizi boyutu büyükse bellek kullanımına dikkat.
    

---

## 🎓 Ekstra Örnek: `$project` ile birleştirme

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      items: { $push: "$product" },
      total: { $sum: "$total" }
    }
  },
  {
    $project: {
      customer: "$_id",
      itemCount: { $size: "$items" },
      total: 1,
      _id: 0
    }
  }
])
```

> ✅ Her müşterinin kaç farklı ürün aldığı ve toplam harcaması gösterilir.

---

## ✅ Özet

|Özellik|`$push`|
|---|---|
|Gruplanan değerleri diziye toplar|✅|
|Aynı değeri tekrar tekrar ekler|✅|
|Sıralı veri sağlar|✅|
|Benzersiz veriler için uygun değil|❌ (→ `$addToSet` kullan)|

---

Hazırsan `$addToSet`, `$first`, `$last` gibi diğer `$group` operatörlerine geçebiliriz, ya da `$sort`, `$unwind`, `$lookup` gibi diğer pipeline aşamalarına geçebiliriz.

Hangisiyle devam etmek istersin?