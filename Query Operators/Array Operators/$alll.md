
Bir array alanının, belirtilen **tüm** değerleri içerip içermediğini kontrol eder.

> Yani: `array.includes(val1) && array.includes(val2) && ...` mantığıdır.

---

## 🔹 1. **Temel Kullanım**

```json
{
  name: "Ali",
  skills: ["MongoDB", "Node.js", "Express"]
}
```

### 👇 Sorgu:

```js
db.users.find({
  skills: { $all: ["MongoDB", "Node.js"] }
})
```

> `skills` dizisi **hem "MongoDB" hem de "Node.js"** içeriyorsa eşleşir.  
> Diğer elemanlar olabilir, önemli değil.

---

## 🔹 2. **Sıralama önemli mi?**

Hayır, `$all` sıralamaya bakmaz. Aşağıdaki tüm veriler eşleşir:

```json
["MongoDB", "Node.js", "Express"]
["Node.js", "MongoDB"]
["MongoDB", "React", "Node.js"]
```

---

## 🔹 3. **Array içindeki Object’lerde `$all` işe yaramaz**

Eğer array şu şekilde objeler içeriyorsa:

```json
tags: [
  { name: "yeni" },
  { name: "kampanya" }
]
```

Şu **çalışmaz**:

```js
db.products.find({
  tags: { $all: ["kampanya", "yeni"] }  // ❌
})
```

### ✅ Bunun yerine:

```js
db.products.find({
  "tags.name": { $all: ["kampanya", "yeni"] }
})
```

> `tags` içinde `name` alanı hem `"kampanya"` hem `"yeni"` olan en az iki obje varsa eşleşir.

---

## 🔹 4. **Sayılarla `$all`**

```json
{
  scores: [85, 90, 100]
}
```

```js
db.students.find({
  scores: { $all: [90, 100] }
})
```

> `scores` dizisi hem `90` hem `100` içeriyorsa eşleşir.

---

## 🔹 5. **`$all` içinde `$elemMatch`**

Eğer her bir öğe objeyse ve birden fazla alan koşulu gerekiyorsa `$all` içinde `$elemMatch` kullanılır:

```json
items: [
  { productId: 101, quantity: 5 },
  { productId: 102, quantity: 10 }
]
```

### 👇 Hem 101 quantity=5 hem 102 quantity=10 varsa:

```js
db.orders.find({
  items: {
    $all: [
      { $elemMatch: { productId: 101, quantity: 5 } },
      { $elemMatch: { productId: 102, quantity: 10 } }
    ]
  }
})
```

> `items` içinde bu iki koşulu sağlayan **iki farklı öğe** varsa eşleşir.

---

## 🔹 6. **Boş `$all: []` kullanımı**

```js
db.collection.find({ skills: { $all: [] } })
```

> Bu tüm belgeleri döndürür. Çünkü "hiçbir şeyin olması beklenmiyor" demektir (true kabul edilir).

---

## ✅ Kısa Özet

|Kullanım|Açıklama|
|---|---|
|`{ tags: { $all: ["mongodb", "nosql"] } }`|Her iki değerin array içinde olması gerekir|
|`{ "tags.name": { $all: ["yeni", "kampanya"] } }`|Objelerdeki name alanı bu iki değeri içermeli|
|`$all + $elemMatch`|Objelerde birden çok alan koşulu sağlanmalı|
|Boş `$all: []`|Tüm belgeler döner|

---

İstersen sıradaki konuyu seçebilirsin:

- 🔸 `$elemMatch` (çok koşullu array elemanı)
    
- 🔸 `$size` (dizi boyutu sorgusu)
    
- 🔸 Array update operatörleri (`$push`, `$pull`, `$addToSet`)
    
- 🔸 Aggregation ile array filtreleme (`$filter`, `$map`)
    

Hangisine geçelim TkMatE?