
## 📁 Koleksiyon: `sales`

```js
db.sales.insertMany([
  { _id: 1, category: "Electronics", price: 1200, quantity: 2, date: ISODate("2025-07-01") },
  { _id: 2, category: "Books", price: 30, quantity: 10, date: ISODate("2025-07-02") },
  { _id: 3, category: "Clothing", price: 100, quantity: 5, date: ISODate("2025-07-03") },
  { _id: 4, category: "Electronics", price: 800, quantity: 1, date: ISODate("2025-07-04") },
  { _id: 5, category: "Books", price: 50, quantity: 4, date: ISODate("2025-07-05") },
  { _id: 6, category: "Electronics", price: 1500, quantity: 1, date: ISODate("2025-07-06") }
])
```

---

## 🎯 Analiz Hedefimiz:

- **1)** Elektronik ürün satışlarını listele (`Electronics`)
- **2)** Kategori bazında toplam satış miktarını bul (`$group`)
- **3)** Satışların ortalama fiyatını hesapla (`$avg`)
- **4)** 1000 TL'den pahalı satış sayısını bul

---

## 🧪 `$facet` ile Sorgu

```js
db.sales.aggregate([
  {
    $facet: {
      electronicSales: [
        { $match: { category: "Electronics" } },
        { $project: { _id: 0, category: 1, price: 1, quantity: 1 } }
      ],
      totalQuantityPerCategory: [
        {
          $group: {
            _id: "$category",
            totalQuantity: { $sum: "$quantity" }
          }
        }
      ],
      averagePrice: [
        {
          $group: {
            _id: null,
            avgPrice: { $avg: "$price" }
          }
        }
      ],
      expensiveSalesCount: [
        { $match: { price: { $gt: 1000 } } },
        { $count: "count" }
      ]
    }
  }
])
```

---

## 🧾 Çıktı (örnek)

```json
[
  {
    "electronicSales": [
      { "category": "Electronics", "price": 1200, "quantity": 2 },
      { "category": "Electronics", "price": 800, "quantity": 1 },
      { "category": "Electronics", "price": 1500, "quantity": 1 }
    ],
    "totalQuantityPerCategory": [
      { "_id": "Electronics", "totalQuantity": 4 },
      { "_id": "Books", "totalQuantity": 14 },
      { "_id": "Clothing", "totalQuantity": 5 }
    ],
    "averagePrice": [
      { "_id": null, "avgPrice": 613.33 }
    ],
    "expensiveSalesCount": [
      { "count": 2 }
    ]
  }
]
```

---

## 📌 Açıklama

|Facet Adı|Açıklama|
|---|---|
|`electronicSales`|Sadece "Electronics" kategorisindeki satışları listeler.|
|`totalQuantityPerCategory`|Her kategori için toplam kaç adet ürün satıldığını gösterir.|
|`averagePrice`|Tüm satışların ortalama fiyatını verir.|
|`expensiveSalesCount`|Fiyatı 1000 TL'den fazla olan satışların sayısını verir.|
