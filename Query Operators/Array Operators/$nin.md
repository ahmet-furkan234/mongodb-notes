
`$nin`, bir alanın değeri, belirtilen değerler **arasında yoksa** eşleşir.  
Yani: `NOT IN` mantığıdır.

---

## 🔹 1. **Temel Kullanım**

```js
db.users.find({
  city: { $nin: ["İstanbul", "Ankara"] }
})
```

> `city` alanı **İstanbul veya Ankara olmayan** tüm kullanıcıları döndürür.

---

## 🔹 2. **Array alanı üzerinde `$nin`**

```json
{
  name: "Ali",
  skills: ["MongoDB", "Node.js", "Express"]
}
```

### 👇 Sorgu:

```js
db.users.find({
  skills: { $nin: ["MongoDB"] }
})
```

> `skills` içinde **"MongoDB" olmayan** belgeleri getirir.

⚠️ Not: Bu şu anlama gelir:

- Ya `skills` alanı hiç yok
    
- Ya `skills` dizisinde `"MongoDB"` yok
    

---

## 🔹 3. **Object Array İçinde `$nin`**

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
  tags: { $nin: ["kampanya"] }  // bu string array için çalışır
})
```

### ✅ Doğru Kullanım:

```js
db.products.find({
  "tags.name": { $nin: ["kampanya"] }
})
```

> `tags` içindeki objelerde `name !== "kampanya"` olan belgeleri döndürür.

---

## 🔹 4. **Sayılarla `$nin`**

```js
db.students.find({
  scores: { $nin: [0, -1] }
})
```

> `scores` dizisinde `0` veya `-1` olmayanları getirir.

---

## 🔹 5. **Boş `$nin: []` kullanımı**

```js
db.users.find({
  city: { $nin: [] }
})
```

> ✅ **Tüm belgeleri döndürür** çünkü hiçbir şey hariç tutulmamış demektir.

---

## 🔹 6. `$nin` + `$elemMatch`

Diyelim ki:

```json
{
  items: [
    { productId: 101, quantity: 2 },
    { productId: 102, quantity: 5 }
  ]
}
```

### 👇 `productId` 101 veya 102 olanları dışla:

```js
db.orders.find({
  items: {
    $elemMatch: {
      productId: { $nin: [101, 102] }
    }
  }
})
```

> `items` array'inde `productId !== 101 && !== 102` olan bir öğe varsa eşleşir.

---

## 🧠 `$nin` ve null davranışı

```js
db.users.find({
  country: { $nin: ["Türkiye"] }
})
```

> `country === null` olanlar da eşleşir. Çünkü `"Türkiye"` yok sayılıyor.

---

## ✅ Özet Tablo

|Kullanım Durumu|Sorgu|
|---|---|
|Alan `"A"` ve `"B"` hariç|`{ field: { $nin: ["A", "B"] } }`|
|Array içinde `"Node.js"` olmayanlar|`{ skills: { $nin: ["Node.js"] } }`|
|Object array’de `name !== "yeni"`|`{ "tags.name": { $nin: ["yeni"] } }`|
|Sayısal değer hariç|`{ scores: { $nin: [0, 100] } }`|

---

İstersen bir sonraki adımda:

- `$elemMatch` (çok koşullu array eşleşmesi)
    
- `$size` (array uzunluğu)
    
- Array güncelleme operatörleri (`$push`, `$pull`, `$addToSet`)
    
- Aggregation array işlemleri (`$filter`, `$map`, `$reduce`)
    

gibi konulardan devam edebiliriz.

Hangisine geçelim TkMatE?