
> Gruplanan belgelerden bir alanın **benzersiz** değerlerini **dizi halinde** toplar.
> SQL karşılığı: `GROUP BY` ile birlikte `DISTINCT` mantığına benzer.

---

## 🔧 Söz Dizimi

```js
$group: {
  _id: "$alan",
  benzersizDizi: { $addToSet: "$alan2" }
}
```

---

## 📁 Örnek Veri Seti: `orders`

```js
db.orders.insertMany([
  { customer: "Ali",  product: "TV",    total: 1000 },
  { customer: "Ali",  product: "Phone", total: 500 },
  { customer: "Ali",  product: "TV",    total: 1200 },
  { customer: "Ayşe", product: "Tablet",total: 800 },
  { customer: "Ayşe", product: "Phone", total: 600 },
  { customer: "Ali",  product: "Phone", total: 300 }
])
```

---

## 📌 1. Her müşterinin benzersiz ürünleri

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      uniqueProducts: { $addToSet: "$product" }
    }
  }
])
```

> ✅ Aynı ürünü birden fazla kez alan kullanıcılar için ürünler tekrarsız olarak gösterilir.

**Sonuç (örnek):**

```json
{
  _id: "Ali",
  uniqueProducts: ["TV", "Phone"]
}
```

---

## 📌 2. Obje olarak `$addToSet` (kompleks kullanım)

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      orderSummary: {
        $addToSet: {
          product: "$product",
          price: "$total"
        }
      }
    }
  }
])
```

> ✅ Her kullanıcı için aynı ürünü farklı fiyata alsa bile **aynı obje** varsa sadece bir kez gelir.

---

## 📌 3. `$addToSet` + `$project` ile liste boyutu gösterme

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$customer",
      uniqueProducts: { $addToSet: "$product" }
    }
  },
  {
    $project: {
      customer: "$_id",
      uniqueCount: { $size: "$uniqueProducts" },
      _id: 0
    }
  }
])
```

> ✅ Her müşterinin kaç farklı ürün aldığı gösterilir.

---

## 📌 4. Tekrar eden ürünleri filtrelemek için `$push` vs `$addToSet`

|Operatör|Açıklama|
|---|---|
|`$push`|Aynı değer tekrar tekrar dizide görünür|
|`$addToSet`|Sadece benzersiz değerleri toplar|

### 🔍 Karşılaştırma:

```js
$push: ["TV", "Phone", "TV"]      // tekrar var
$addToSet: ["TV", "Phone"]        // tekrar yok
```

---

## 📌 5. Filtrelenmiş veriyle kullanma (`$match` ile)

```js
db.orders.aggregate([
  { $match: { total: { $gte: 600 } } },
  {
    $group: {
      _id: "$customer",
      bigItems: { $addToSet: "$product" }
    }
  }
])
```

> ✅ 600 ve üzeri fiyatlı ürünleri alan kullanıcılar, tekrarsız olarak listelenir.

---

## 🧠 Teknik Bilgi

- Dizi boyutu sınırı vardır (16 MB doküman sınırı).
    
- Sıralama garantisi **yoktur**, çünkü `set` yapısı **sırasızdır**.
    

---

## 🎓 Gerçek Hayat Senaryosu

> "Her öğrencinin katıldığı benzersiz kulüpleri listele"

```js
db.students.insertMany([
  { name: "Ahmet", club: "Basketball" },
  { name: "Ahmet", club: "Chess" },
  { name: "Ahmet", club: "Basketball" },
  { name: "Zeynep", club: "Chess" },
  { name: "Zeynep", club: "Math" }
])

db.students.aggregate([
  {
    $group: {
      _id: "$name",
      clubs: { $addToSet: "$club" }
    }
  }
])
```

---

## ✅ Özet

|Özellik|`$addToSet`|
|---|---|
|Gruplanan veriyi diziye ekler|✅|
|Yineleyen değerleri atar|✅|
|Obje şeklinde veri eklenebilir|✅|
|Sıralı dizi oluşturmaz|❌ (→ `$push`)|

---

Hazırsan şimdi **`$first`** ve **`$last`** gibi sıralamaya bağlı özel `$group` operatörlerine geçebiliriz, ya da `$unwind`, `$sort`, `$lookup` gibi pipeline aşamalarına dönebiliriz.

Hangisiyle devam edelim TkMatE?