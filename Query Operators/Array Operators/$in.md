
`$in`, bir alanın değeri **belirttiğin bir dizi içindeki herhangi bir değerle eşleşiyor mu?** sorusunu sorar. Bu değerler hem düz değerlerde hem de array içindeki elemanlarda çalışır.

---

## 🔹 1. **Temel Kullanım:**

### 👇 Sorgu:

```js
db.users.find({
  city: { $in: ["Ankara", "İstanbul"] }
})
```

> `city` alanı **Ankara veya İstanbul** olan tüm kullanıcıları getirir.

---

## 🔹 2. **Array içinde `$in` kullanımı**

```json
{
  name: "Ali",
  skills: ["MongoDB", "Node.js", "Express"]
}
```

### 👇 Sorgu:

```js
db.users.find({
  skills: { $in: ["Node.js"] }
})
```

> `skills` dizisinde `"Node.js"` olanları getirir.

> Yani **bir array alanının içinde verilen değeri arar**.

---

## 🔹 3. **Çoklu Değer Arama**

```js
db.products.find({
  category: { $in: ["Elektronik", "Telefon"] }
})
```

> `category` alanı bu iki değerden herhangi biriyle eşleşiyorsa döner.

---

## 🔹 4. **Array İçindeki Objelerin Belirli Alanlarıyla `$in` Kullanımı**

```json
{
  name: "Ürün A",
  tags: [
    { name: "kampanya" },
    { name: "yeni" }
  ]
}
```

### ❌ Bu işe yaramaz:

```js
db.products.find({
  tags: { $in: ["kampanya"] }  // bu sadece string array için çalışır
})
```

### ✅ Doğru kullanım (`tags.name`):

```js
db.products.find({
  "tags.name": { $in: ["kampanya"] }
})
```

> `tags` içinde `name == "kampanya"` olan herhangi bir obje varsa eşleşir.

---

## 🔹 5. **Numerik Değerlerle `$in`**

```json
{
  _id: 1,
  scores: [70, 85, 95]
}
```

```js
db.students.find({
  scores: { $in: [85, 100] }
})
```

> `scores` içinde 85 veya 100 olan öğrenciler döner.

---

## 🔹 6. **Not: `$in` ile boş array çalışmaz**

```js
db.users.find({
  skills: { $in: [] }
})
```

> ❌ Bu sorgu **hiçbir belge döndürmez** çünkü `$in: []` her zaman `false` döner.

---

## 🔹 7. `$in` ile `$regex` kullanımı

```js
db.products.find({
  name: { $in: [/^SAMSUNG/i, /^APPLE/i] }
})
```

> `name` alanı "SAMSUNG" veya "APPLE" ile başlayan ürünleri döndürür.

---

## 🔹 8. `$in` ile birlikte `$elemMatch` kullanımı (karmaşık yapılar için)

```js
db.orders.find({
  items: {
    $elemMatch: {
      productId: { $in: [101, 102, 103] },
      quantity: { $gte: 5 }
    }
  }
})
```

> `items` içinde, `productId` değeri 101, 102 veya 103 olan ve `quantity >= 5` olan en az bir öğe varsa eşleşir.

---

## ✅ Özet Tablo

|Kullanım Durumu|Sorgu|
|---|---|
|Alan eşleşmesi|`{ city: { $in: ["İstanbul", "İzmir"] } }`|
|Dizi içinde değer|`{ skills: { $in: ["MongoDB"] } }`|
|Object array'de alan eşleşmesi|`{ "tags.name": { $in: ["yeni"] } }`|
|Regex ile eşleşme|`{ name: { $in: [/^SAMSUNG/, /^APPLE/] } }`|
