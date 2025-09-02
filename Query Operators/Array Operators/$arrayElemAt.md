
```js
$arrayElemAt: [ <dizi>, <indeks> ]
```

- Belirtilen dizinin belirli indeksindeki öğeyi getirir.
- İndeks `0` tabanlıdır. Yani ilk eleman `0`, ikinci `1`, vb.
- Negatif indeks yazarsan sondan başa doğru sayar.

---

## 🧪 Basit Örnek

```js
db.example.aggregate([
  {
    $project: {
      firstHobby: { $arrayElemAt: ["$hobbies", 0] }
    }
  }
])
```

### 📁 Belge:

```json
{
  "_id": 1,
  "name": "Ali",
  "hobbies": ["reading", "swimming", "coding"]
}
```

### 🧾 Çıktı:

```json
{
  "_id": 1,
  "firstHobby": "reading"
}
```

---

## 🔁 Negatif İndeks

```js
$arrayElemAt: ["$hobbies", -1]
```

➡️ Bu da **son elemanı** verir (`"coding"`)

---

## 💥 `$facet` ile Birlikte Kullanımı

Bunu genellikle `$facet` ile gelen dizilerden **tekil değerleri çıkarmak için** kullanırsın:

### 🎯 Örnek:

```js
{
  $addFields: {
    averageScore: { $arrayElemAt: ["$averageScore.avg", 0] }
  }
}
```

Bu örnekte:

- `averageScore` dizisi içindeki ilk elemandaki `"avg"` alanını alıyoruz.
    

---

## 🛑 Hata Durumu

Eğer index değerin dizinin uzunluğundan büyükse veya dizi yoksa:

```json
{ "$arrayElemAt": [null, 0] }
```

➡️ `null` döner. Hata vermez ama veri gelmez.

---

## 🧪 Gerçek Senaryo

```js
db.orders.aggregate([
  {
    $facet: {
      totalAmount: [
        { $group: { _id: null, total: { $sum: "$amount" } } }
      ]
    }
  },
  {
    $addFields: {
      totalAmount: { $arrayElemAt: ["$totalAmount.total", 0] }
    }
  },
  {
    $project: {
      _id: 0,
      totalAmount: 1
    }
  }
])
```

### 🎯 Sonuç:

```json
{
  "totalAmount": 19500
}
```

---

## 🛠 İleri Kullanım: `$map`, `$filter` ile birlikte

```js
{
  $arrayElemAt: [
    { $filter: {
        input: "$items",
        as: "item",
        cond: { $gt: ["$$item.price", 100] }
    }},
    0
  ]
}
```

➡️ Bu örnek: `price > 100` olan ilk öğeyi getirir.

---

## ✅ Kapanış

|Özellik|Açıklama|
|---|---|
|Veri türü|Dizi (array)|
|Geri dönüş|Dizi elemanı|
|Hatalı index|`null` döner|
|En yaygın kullanım|`$facet`, `$group`, `$bucket` sonrası dizi çözme|

---

İstersen şimdi `$slice`, `$filter` veya `$map` gibi **dizi fonksiyonları**na geçebiliriz.  
Hangisiyle devam edelim TkMatE?