

```js
$facet: {
  <çıktıAdı1>: [ <pipelineAşamaları> ],
  <çıktıAdı2>: [ <pipelineAşamaları> ],
  ...
}
```

- `$facet`, **her bir alan için bağımsız pipeline çalıştırır**.
- Çıktılar tek bir **belgede birleştirilmiş dizi alanları** olarak döner.

---

## 📊 Gerçek Hayat Senaryosu: Ürün Verisi Üzerinden Analiz

Bir `products` koleksiyonumuz olsun:

```json
{
  "_id": 1,
  "name": "Laptop",
  "category": "Electronics",
  "price": 1000
}
```

### 🎯 Amaç: Aşağıdaki analizleri aynı anda yapmak:

1. Fiyatı 1000'den büyük ürün sayısı
2. Kategorilere göre ürün sayısı
3. Ortalama fiyat

---

## ✅ `$facet` Kullanımı

```js
db.products.aggregate([
  {
    $facet: {
      expensiveProducts: [
        { $match: { price: { $gt: 1000 } } },
        { $count: "count" }
      ],
      categoryBreakdown: [
        { $group: { _id: "$category", count: { $sum: 1 } } }
      ],
      averagePrice: [
        {
          $group: {
            _id: null,
            avgPrice: { $avg: "$price" }
          }
        }
      ]
    }
  }
])
```

---

### 📥 Dönüş Şu Şekilde Olur:

```json
[
  {
    "expensiveProducts": [{ "count": 5 }],
    "categoryBreakdown": [
      { "_id": "Electronics", "count": 10 },
      { "_id": "Books", "count": 7 }
    ],
    "averagePrice": [{ "_id": null, "avgPrice": 750 }]
  }
]
```

---

## 🧩 Nerede Kullanılır?

- **Dashboard’lar**: Tek sorguda birden fazla metrik göstermek için.
- **Çoklu filtreli sorgular**: Kullanıcıya aynı anda farklı breakdown’lar sunmak için.
- **Veri analizleri**: Detaylı raporlama işlemlerinde.

---

## 🧪 Küçük Örnek: Öğrenci Not Analizi

```js
db.students.aggregate([
  {
    $facet: {
      passed: [
        { $match: { grade: { $gte: 50 } } },
        { $count: "count" }
      ],
      failed: [
        { $match: { grade: { $lt: 50 } } },
        { $count: "count" }
      ],
      averageGrade: [
        { $group: { _id: null, avg: { $avg: "$grade" } } }
      ]
    }
  }
])
```

---

## 📝 Notlar

- `$facet`, **pipeline içinde genellikle sonlara doğru** kullanılır.
- Her facet’in sonucu, **bir dizi (`array`)** olarak döner.
- Hafif sorgularla birlikte kullanılmalı çünkü her facet **bağımsız çalışır ve işlem yükü oluşturabilir**.
