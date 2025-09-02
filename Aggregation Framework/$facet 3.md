
## ✅ Hedef: `$facet` sonrası tekil alanları doğrudan erişilebilir hale getirmek

Önceki örnekteki şu sonucu düşün:

```json
"averagePrice": [ { "_id": null, "avgPrice": 613.33 } ]
```

Bunu şöyle istiyorsun:

```json
"averagePrice": 613.33
```

---

## 🧪 Çözüm 1: `$addFields` + `$arrayElemAt`

```js
db.sales.aggregate([
  {
    $facet: {
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
  },
  {
    $addFields: {
      averagePrice: { $arrayElemAt: ["$averagePrice.avgPrice", 0] },
      expensiveSalesCount: { $arrayElemAt: ["$expensiveSalesCount.count", 0] }
    }
  },
  {
    $project: {
      _id: 0,
      averagePrice: 1,
      expensiveSalesCount: 1
    }
  }
])
```

---

## 🧾 Örnek Çıktı:

```json
{
  "averagePrice": 613.33,
  "expensiveSalesCount": 2
}
```

---

## 🔍 Açıklama

|Operatör|Ne Yaptı?|
|---|---|
|`$arrayElemAt`|Dizi içindeki ilk elemanı aldı (`[0]`)|
|`$addFields`|Yeni alanlar olarak eklendi|
|`$project`|Gereksiz alanlar (`_id`, facet dizileri) gizlendi|

---

## 💡 Alternatif: `$unwind` kullanarak dizi açma

Eğer dizide sadece 1 eleman varsa:

```js
{ $unwind: "$averagePrice" }
{ $replaceRoot: { newRoot: { $mergeObjects: ["$$ROOT", "$averagePrice"] } } }
```

Ama `$arrayElemAt` kullanımı **daha kontrollü ve güvenli** olur.

---

İstersen bu yapıyı çok daha kapsamlı dashboard yapısı gibi kullanabileceğin şekilde genişletip, tüm verileri sadeleştirebilirim.

Devam etmek ister misin?