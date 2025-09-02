
`$facet` ile oluşturduğun **birden fazla pipeline çıktısı tek bir belge içinde ayrı diziler olarak döner**. Bunlara erişmek için sonrasında:

- **`$project`** veya   
- **`$addFields` (ya da `$set`)**  kullanarak bu dizilerin içinden istediğin alanları çıkarabilir, tekil değer haline getirebilir veya sadeleştirebilirsin.

---

## Nasıl yapacağın adım adım:

### 1) `$facet` çıktı yapısı:

```json
{
  "pipeline1": [ {...}, {...}, ... ],
  "pipeline2": [ {...} ],
  "pipeline3": [ {...}, {...} ]
}
```

Her alan bir **dizi (array)** döner.

---

### 2) Örnek: Diziden tek değer almak

Örneğin, `pipeline2` dizisi tek bir eleman içeriyor ve içinde `"count"` alanı var.

```js
{
  $addFields: {
    pipeline2Count: { $arrayElemAt: ["$pipeline2.count", 0] }
  }
}
```

Bu şekilde:

```json
"pipeline2Count": 42
```

şeklinde düz bir alan elde edersin.

---

### 3) `$project` ile gereksiz dizileri kaldırmak

Son aşamada:

```js
{
  $project: {
    pipeline1: 0,
    pipeline2: 0,
    pipeline3: 0
  }
}
```

yaparak dizileri gizleyip sadece istediğin sade alanları gösterebilirsin.

---

## Tam örnek

```js
db.collection.aggregate([
  {
    $facet: {
      expensiveItems: [
        { $match: { price: { $gt: 1000 } } },
        { $count: "count" }
      ],
      averagePrice: [
        { $group: { _id: null, avgPrice: { $avg: "$price" } } }
      ]
    }
  },
  {
    $addFields: {
      expensiveCount: { $arrayElemAt: ["$expensiveItems.count", 0] },
      avgPrice: { $arrayElemAt: ["$averagePrice.avgPrice", 0] }
    }
  },
  {
    $project: {
      expensiveItems: 0,
      averagePrice: 0
    }
  }
])
```

---

## Özet

- `$facet` diziler döner.
- `$arrayElemAt` ile diziden eleman çıkarırsın.
- `$addFields`/`$set` ile yeni alanlara atarsın.
- Son `$project` ile gereksiz dizileri gizlersin.
