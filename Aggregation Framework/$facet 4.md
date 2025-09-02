
MongoDB’de `$facet` operatörünün **köşeli parantez (`[ ]`) içindeki her süslü blok (`{ ... }`) aslında **bağımsız bir pipeline aşamasıdır.****

---

## 📦 `$facet` Yapısı

```js
{
  $facet: {
    birinciAnaliz: [   // ⬅️ Bu bir pipeline
      { $match: {...} },
      { $group: {...} },
      { $project: {...} }
    ],
    ikinciAnaliz: [    // ⬅️ Bu da başka bir pipeline
      { $match: {...} },
      { $count: "adet" }
    ]
  }
}
```

🧠 Her `anahtar: [ ... ]` tanımı bir **mini aggregation pipeline**’dır ve **hepsi aynı veri kümesinden bağımsız çalışır**.

---

## 📌 Örnekle Gösterelim

```js
db.sales.aggregate([
  {
    $facet: {
      expensiveSales: [
        { $match: { price: { $gt: 1000 } } },
        { $count: "count" }
      ],
      categorySummary: [
        { $group: { _id: "$category", total: { $sum: "$price" } } }
      ]
    }
  }
])
```

### 💡 Burada:

- `expensiveSales` → kendi içinde 2 aşamalı bir pipeline
- `categorySummary` → kendi içinde 1 aşamalı bir pipeline

🧩 Bu iki analiz birbirinden **bağımsız olarak aynı veri kümesinde çalışır.**

---

## 🔬 Nasıl İşler?

1. MongoDB, `$facet`'e girildiğinde tüm belgeleri belleğe alır.
2. Her `[ ... ]` pipeline paralel şekilde çalıştırılır.
3. Sonuç tek bir belge içinde aşağıdaki gibi döner:
    

```json
{
  "expensiveSales": [ { "count": 2 } ],
  "categorySummary": [
    { "_id": "Electronics", "total": 3500 },
    { "_id": "Books", "total": 80 }
  ]
}
```

---

## ✅ Özet

|Yapı|Anlamı|
|---|---|
|`$facet: { key: [ { ... } ] }`|Her `key` bir mini pipeline|
|`[ { ... }, { ... } ]`|Aşamalar (stage'ler)|
|Her facet birbirinden bağımsız|Evet, paralel çalışırlar|
|Hepsi aynı veri kümesini kullanır|Evet, `$facet` öncesindeki sonucu|
