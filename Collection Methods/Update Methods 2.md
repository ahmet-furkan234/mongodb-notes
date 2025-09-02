
## 1. `$set` — Alan Değerini Ayarlama / Güncelleme

Belirtilen alanın değerini değiştirir veya yoksa ekler.

```js
db.users.updateOne(
  { username: "ayse" },
  { $set: { city: "İstanbul" } }
)
```

> `username: "ayse"` olan kullanıcının `city` alanı "İstanbul" olarak güncellenir.

---

## 2. `$unset` — Alanı Belgeden Kaldırma

Belgedeki alanı tamamen siler.

```js
db.users.updateOne(
  { username: "ahmet" },
  { $unset: { age: "" } }
)
```

> `ahmet` kullanıcısının `age` alanı belgede artık olmayacak.

---

## 3. `$inc` — Sayısal Alanı Arttırma veya Azaltma

Sayısal alanın değerini belirtilen miktarda artırır veya azaltır.

```js
db.users.updateOne(
  { username: "mehmet" },
  { $inc: { loginCount: 1 } }
)
```

> `loginCount` sayısını 1 artırır.

```js
db.products.updateOne(
  { _id: 101 },
  { $inc: { stock: -5 } }
)
```

> Stok miktarını 5 azaltır.

---

## 4. `$push` — Diziye Eleman Ekleme

Dizi tipindeki alana yeni bir eleman ekler.

```js
db.users.updateOne(
  { username: "ali" },
  { $push: { hobbies: "basketbol" } }
)
```

> `ali` kullanıcısının `hobbies` dizisine "basketbol" eklenir.

### `$push` ile çoklu eleman ekleme:

```js
db.users.updateOne(
  { username: "ali" },
  { $push: { hobbies: { $each: ["kitap", "müzik"] } } }
)
```

---

## 5. `$addToSet` — Diziye Benzersiz Eleman Ekleme

Eğer eleman zaten varsa eklemez, yoksa ekler (tekrarı önler).

```js
db.users.updateOne(
  { username: "ayse" },
  { $addToSet: { tags: "premium" } }
)
```

---

## 6. `$pop` — Diziden Eleman Silme

- `-1` : Dizinin ilk elemanını siler.
- `1` : Dizinin son elemanını siler.

```js
db.users.updateOne(
  { username: "ali" },
  { $pop: { hobbies: -1 } }  // İlk eleman silinir
)
```

---

## 7. `$rename` — Alan Adını Değiştirme

Mevcut alanın adını değiştirir.

```js
db.users.updateOne(
  { username: "mehmet" },
  { $rename: { "oldField": "newField" } }
)
```

---

## 8. `$mul` — Sayısal Alanı Çarpma

Sayısal bir alanı belirtilen değerle çarpar.

```js
db.products.updateOne(
  { _id: 100 },
  { $mul: { price: 1.1 } }  // fiyatı %10 artırır
)
```

---

## 9. `$max` — Alan Değerini Belirtilen Değere Eşitler, ancak sadece mevcut değerden büyükse

```js
db.users.updateOne(
  { username: "ahmet" },
  { $max: { score: 100 } }
)
```

> `score` 100'den küçükse 100 yapılır, büyükse değiştirilmez.

---

## 10. `$min` — Alan Değerini Belirtilen Değere Eşitler, ancak sadece mevcut değerden küçükse

```js
db.users.updateOne(
  { username: "ayse" },
  { $min: { score: 50 } }
)
```

> `score` 50'den büyükse 50 yapılır, küçükse değiştirilmez.

---

# Karmaşık Güncelleme Örneği

```js
db.users.updateOne(
  { username: "ali" },
  {
    $set: { city: "Ankara" },
    $inc: { loginCount: 1 },
    $addToSet: { tags: "vip" },
    $unset: { tempField: "" }
  }
)
```

> `ali` kullanıcısının:
> - `city` alanı "Ankara" olarak güncellenir,
> - `loginCount` 1 artırılır,
> - `tags` dizisine "vip" eklenir (varsa eklemez),
> - `tempField` alanı kaldırılır.
