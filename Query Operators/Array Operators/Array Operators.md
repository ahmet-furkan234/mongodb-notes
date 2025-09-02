
Aşağıda en yaygın kullanılan array operatörlerini bulacaksın:

| Operatör     | Açıklama                                                                                  |
| ------------ | ----------------------------------------------------------------------------------------- |
| `$all`       | Tüm belirtilen değerleri array içinde arar                                                |
| `$elemMatch` | Array içindeki objeler için çok koşullu eşleşme yapar                                     |
| `$size`      | Dizinin eleman sayısına göre sorgular                                                     |
| `$in`        | Dizide belirtilen değerlerden herhangi biri varsa döner                                   |
| `$nin`       | Dizide belirtilen değerlerden hiçbiri yoksa döner                                         |
| `$slice`     | Dizi içinden belirli sayıda eleman alır (genellikle `find` + `projection` ile kullanılır) |

---

## 🔹 1. `$all` – Hepsi varsa döner

Array içinde **birden fazla değerin hepsi** varsa eşleşir.

```js
// tags: ["mongodb", "nosql", "database"]
db.collection.find({
  tags: { $all: ["mongodb", "nosql"] }
})
```

> Hem "mongodb" hem "nosql" varsa döner.

---

## 🔹 2. `$elemMatch` – Array içindeki objeye **çoklu koşul**

Array içindeki bir **tek obje**, verilen tüm koşulları sağlamak zorundadır.

```js
// items: [{name: "elma", price: 10}, {name: "muz", price: 20}]
db.collection.find({
  items: { $elemMatch: { name: "muz", price: { $gt: 15 } } }
})
```

> `items` dizisinde `name == "muz"` ve `price > 15` olan aynı obje varsa eşleşir.

---

## 🔹 3. `$size` – Dizinin eleman sayısını kontrol eder

```js
db.collection.find({ tags: { $size: 3 } })
```

> `tags` dizisi **tam olarak 3 elemanlıysa** döner.

---

## 🔹 4. `$in` – En az bir eşleşme varsa döner

```js
db.collection.find({ tags: { $in: ["mongodb", "mysql"] } })
```

> `tags` dizisinde bu iki değerden en az biri varsa döner.

---

## 🔹 5. `$nin` – Hiçbiri olmamalı

```js
db.collection.find({ tags: { $nin: ["mysql", "oracle"] } })
```

> `tags` dizisi bu iki değerden **hiçbirini** içermemeli.

---

## 🔹 6. `$slice` – Array'den sadece belirli bir kısmı

Bu bir **projection operatörüdür**, yani veri alırken sadece belli parçayı getirir.

```js
db.collection.find(
  {},
  { tags: { $slice: 2 } }
)
```

> `tags` dizisinden ilk 2 elemanı getirir.

```js
db.collection.find(
  {},
  { tags: { $slice: [-2] } }
)
```

> Son 2 elemanı getirir.

---

## 💡 Örnek Veri

```json
{
  _id: 1,
  tags: ["mongodb", "nosql", "database"],
  items: [
    { name: "elma", price: 10 },
    { name: "muz", price: 20 }
  ]
}
```

|Operatör|Örnek|
|---|---|
|`$all`|`{ tags: { $all: ["mongodb", "nosql"] } }`|
|`$elemMatch`|`{ items: { $elemMatch: { name: "elma", price: 10 } } }`|
|`$size`|`{ tags: { $size: 3 } }`|
|`$in`|`{ tags: { $in: ["nosql"] } }`|
|`$nin`|`{ tags: { $nin: ["sql"] } }`|
|`$slice`|`{ tags: { $slice: [-1] } }` → sadece son eleman|

---

İstersen sıradaki aşamada:

✅ `$filter` operatörü ile aggregation'da array süzme  
✅ `$map` ile array'i dönüştürme  
✅ `$reduce` ile array'den tek değer çıkarma  
✅ `$push`, `$addToSet`, `$pull` gibi **update array operatörleri**

gibi daha **ileri seviye array işlemlerine** geçebiliriz.

Hangisinden devam edelim TkMatE?