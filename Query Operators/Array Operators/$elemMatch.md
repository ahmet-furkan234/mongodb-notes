
Bir **array içinde tek bir öğe** bulmak istiyorsan ve o öğenin **birden fazla koşulu sağlaması** gerekiyorsa `$elemMatch` kullanılır.

> `skills: { $elemMatch: { type: "backend", level: "expert" } }` gibi.

---

## 🔹 1. **Neden `$elemMatch` gerekir?**

```json
{
  items: [
    { name: "elma", price: 10 },
    { name: "muz", price: 20 }
  ]
}
```

### ❌ Bu sorgu beklediğin gibi çalışmaz:

```js
db.collection.find({
  "items.name": "elma",
  "items.price": 20
})
```

> Bu, **elma olan bir item** ve **price’ı 20 olan başka bir item** varsa eşleşir.

---

### ✅ Tek bir öğe içinde **her iki koşul da sağlanmalı** dersen:

```js
db.collection.find({
  items: {
    $elemMatch: { name: "elma", price: 10 }
  }
})
```

> Bu sorgu, `items` array'i içinde hem `name: "elma"` **ve** `price: 10` olan **tek bir öğe** varsa eşleşir.

---

## 🔹 2. **Dizi içindeki sayısal değerlerde**

```json
{
  scores: [55, 67, 80]
}
```

### Örnek: 60 < score < 70

```js
db.students.find({
  scores: {
    $elemMatch: { $gt: 60, $lt: 70 }
  }
})
```

> `scores` içinde hem `>60` hem `<70` olan **tek bir değer** varsa eşleşir.

---

## 🔹 3. **Object array içinde çoklu koşul örneği**

```json
{
  exams: [
    { subject: "Math", grade: 90 },
    { subject: "History", grade: 85 }
  ]
}
```

### Örnek: subject = "Math" ve grade > 80

```js
db.students.find({
  exams: {
    $elemMatch: { subject: "Math", grade: { $gt: 80 } }
  }
})
```

---

## 🔹 4. **`$all` ile `$elemMatch` birlikte kullanımı**

```json
{
  items: [
    { productId: 1, qty: 2 },
    { productId: 2, qty: 5 }
  ]
}
```

### Her iki ürünü de belirli koşullarla birlikte kontrol:

```js
db.orders.find({
  items: {
    $all: [
      { $elemMatch: { productId: 1, qty: 2 } },
      { $elemMatch: { productId: 2, qty: 5 } }
    ]
  }
})
```

> Hem `productId: 1 & qty: 2` hem `productId: 2 & qty: 5` içeren kayıtlar döner.

---

## 🔹 5. **Array içi nesne yerine düz değerle kullanım**

```json
{
  numbers: [1, 2, 3, 4, 5]
}
```

```js
db.collection.find({
  numbers: { $elemMatch: { $gt: 3, $lt: 5 } }
})
```

> **Tek bir sayı** 3 ile 5 arasında olmalı. (Yani `4` varsa eşleşir.)

---

## ✅ Özet Tablo

|Kullanım|Açıklama|
|---|---|
|`$elemMatch: { a: 1, b: 2 }`|a=1 **ve** b=2 olan **tek öğe** aranır|
|`$elemMatch: { $gt: 10, $lt: 20 }`|10 < x < 20 olan **tek bir değer** aranır|
|`$all + $elemMatch`|Birden fazla öğe eşleşmesi isteniyorsa|

---

## 💡 Neden Önemli?

Sorguyu şu iki şekilde düşün:

|Sorgu Tipi|Ne Anlama Geliyor|
|---|---|
|`field.subfield1 = x` **ve** `field.subfield2 = y`|Farklı objeler olabilir (eşleşme zayıf)|
|`$elemMatch: { subfield1 = x, subfield2 = y }`|Aynı obje olmalı (güçlü eşleşme)|

---

İstersen sıradaki başlık olarak:

- `$size` (array uzunluğunu kontrol)
    
- `$slice` (array'den belirli kısmı alma – projection)
    
- Array update operatörleri (`$push`, `$pull`, `$addToSet`)
    
- Aggregation array operatörleri (`$filter`, `$map`)
    

gibi konulardan devam edebiliriz.

Hangisine geçelim TkMatE?