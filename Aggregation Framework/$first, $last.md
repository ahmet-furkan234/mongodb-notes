
- **`$first`**: Gruplanan belgeler içinde **ilk belge**deki alanın değerini alır.
- **`$last`**: Gruplanan belgeler içinde **son belge**deki alanın değerini alır.

> ❗ Bu operatörlerin doğru çalışabilmesi için **öncesinde `$sort` aşamasıyla sıralama yapılmalıdır**.

---

## 📁 Örnek Veri Seti: `orders`

```js
db.orders.insertMany([
  { customer: "Ali",  product: "TV",    total: 1000, date: ISODate("2024-01-10") },
  { customer: "Ali",  product: "Phone", total: 500,  date: ISODate("2024-02-01") },
  { customer: "Ali",  product: "Laptop",total: 2000, date: ISODate("2024-03-01") },
  { customer: "Ayşe", product: "Tablet",total: 800,  date: ISODate("2024-01-05") },
  { customer: "Ayşe", product: "Phone", total: 600,  date: ISODate("2024-04-15") }
])
```

---

## 📌 1. Her müşterinin **ilk aldığı ürün**

```js
db.orders.aggregate([
  { $sort: { customer: 1, date: 1 } },  // tarih bazlı sıralama
  {
    $group: {
      _id: "$customer",
      firstProduct: { $first: "$product" }
    }
  }
])
```

> ✅ İlk siparişe göre gruplar:
> - Ali → "TV"
> - Ayşe → "Tablet"

---

## 📌 2. Her müşterinin **son aldığı ürün**

```js
db.orders.aggregate([
  { $sort: { customer: 1, date: 1 } },  // tarih bazlı sıralama
  {
    $group: {
      _id: "$customer",
      lastProduct: { $last: "$product" }
    }
  }
])
```

> ✅ Son siparişe göre gruplar:
> - Ali → "Laptop"
> - Ayşe → "Phone"

---

## 📌 3. `$first` ve `$last` aynı anda kullanmak

```js
db.orders.aggregate([
  { $sort: { customer: 1, date: 1 } },
  {
    $group: {
      _id: "$customer",
      firstProduct: { $first: "$product" },
      lastProduct: { $last: "$product" },
      firstDate: { $first: "$date" },
      lastDate: { $last: "$date" }
    }
  }
])
```

> ✅ Sipariş tarih aralığı + ilk ve son ürün bilgisi birlikte döner.

---

## 📌 4. Sıralama olmadan `$first` ve `$last` kullanılırsa?

> MongoDB varsayılan belge sırasına göre sonuç döndürür (bu sıra **güvenilir değildir**).  
> Bu yüzden her zaman:

✅ Önce **`$sort`**, sonra **`$group` + `$first`/`$last`**

---

## 📌 5. Farklı alanları birlikte almak

```js
db.orders.aggregate([
  { $sort: { date: 1 } },
  {
    $group: {
      _id: "$customer",
      firstOrder: {
        $first: {
          product: "$product",
          total: "$total"
        }
      },
      lastOrder: {
        $last: {
          product: "$product",
          total: "$total"
        }
      }
    }
  }
])
```

> ✅ İlk ve son siparişi obje olarak verir.

---

## 💡 `$first` ve `$last` kullanımları için örnek senaryolar

|Senaryo|Operatör|
|---|---|
|İlk sipariş alınan ürün|`$first`|
|Son giriş yapılan tarih|`$last`|
|En eski/son eklenti açıklaması|`$first` / `$last`|
|En düşük tarihli log mesajı|`$first`|
|Son yapılan işlem türü|`$last`|

---

## 🧠 Teknik Notlar

- Sıralamayı doğru yapmazsan yanlış sonuç alırsın.
- Genellikle `$sort` ve `$group` birlikte çalışır.
- Obje içinde değer alabilir (`$first: {field: "$name", date: "$date"}` değil ama `$first: "$name"` → `$first` dışarıda olmalı).

---

## ✅ Özet

|Özellik|`$first`|`$last`|
|---|---|---|
|İlk belgeyi alır|✅|❌|
|Son belgeyi alır|❌|✅|
|Sıralamaya bağlı mı?|✅ (şiddetle evet!)|✅|
|Obje dönebilir mi?|✅|✅|
