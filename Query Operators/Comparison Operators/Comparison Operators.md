
| Operatör | Açıklama                           | SQL Karşılığı |
| -------- | ---------------------------------- | ------------- |
| `$eq`    | Eşit (equal)                       | `=`           |
| `$ne`    | Eşit değil (not equal)             | `!=`          |
| `$gt`    | Büyük (greater than)               | `>`           |
| `$gte`   | Büyük eşit (greater than or equal) | `>=`          |
| `$lt`    | Küçük (less than)                  | `<`           |
| `$lte`   | Küçük eşit (less than or equal)    | `<=`          |

---

## 🔹 `$eq` – Eşit

```js
db.ogrenciler.find({ yas: { $eq: 20 } })
```

Aynı işlemi şu şekilde de yazabilirsin (kısa hali):

```js
db.ogrenciler.find({ yas: 20 })
```

---

## 🔹 `$ne` – Eşit Değil

```js
db.ogrenciler.find({ yas: { $ne: 18 } })
```

> 18 yaşında **olmayan** öğrenciler döner.

---

## 🔹 `$gt` – Büyük

```js
db.ogrenciler.find({ yas: { $gt: 25 } })
```

> Yaşı 25’ten **büyük** olan öğrenciler.

---

## 🔹 `$gte` – Büyük Eşit

```js
db.ogrenciler.find({ yas: { $gte: 18 } })
```

> Yaşı 18 veya daha büyük öğrenciler.

---

## 🔹 `$lt` – Küçük

```js
db.ogrenciler.find({ yas: { $lt: 15 } })
```

> 15’ten **küçük** yaşlar.

---

## 🔹 `$lte` – Küçük Eşit

```js
db.ogrenciler.find({ yas: { $lte: 20 } })
```

> 20 ve altı yaşlar.

---

## 🔸 Birden Fazla Comparison Operatör

```js
db.ogrenciler.find({
  yas: { $gte: 18, $lte: 25 }
})
```

> 18 ile 25 **arasında** (dahil) olanlar.

---

## 🔸 Birden Fazla Alan Üzerinde

```js
db.ogrenciler.find({
  yas: { $gte: 18 },
  ortalama: { $gt: 75 }
})
```

> Yaşı en az 18 **ve** ortalaması 75’ten büyük öğrenciler.

---

## 🔸 Nested (İç İçe) Belgelerde Kullanım

```js
db.sinavlar.find({
  "notlar.mat": { $gt: 80 }
})
```

> `notlar` içinde `mat` (matematik) alanı 80’den büyük olan belgeler.

---

## 🔸 Dizi Üzerinde Kullanım

Eğer bir alan dizi ise `$gt`, `$lt` gibi operatörler, dizinin **her bir elemanı** üzerinde çalışır.

```js
db.kitaplar.find({
  puanlar: { $gt: 4.5 }
})
```

> `puanlar` dizisinde herhangi bir değer 4.5’ten büyükse eşleşir.

---

## 🛠 SQL Karşılaştırması

|SQL Sorgusu|MongoDB Karşılığı|
|---|---|
|`SELECT * FROM ogrenci WHERE yas = 20`|`{ yas: 20 }`|
|`WHERE yas != 18`|`{ yas: { $ne: 18 } }`|
|`WHERE yas BETWEEN 18 AND 25`|`{ yas: { $gte: 18, $lte: 25 } }`|
|`WHERE puan > 85 AND yas < 20`|`{ puan: { $gt: 85 }, yas: { $lt: 20 } }`|

---

## 🧪 Challenge: Uygulamalı 10 Örnek

Aşağıdakileri uygulamak ister misin?

1. Yaşı 20 olanları getir
2. Yaşı 20 **olmayanları** getir
3. Ortalama puanı 75’ten **büyük** olanları getir
4. 18 yaşından **küçük** öğrencileri getir
5. 20 ile 30 arasında olanları getir
6. Mat notu 80’den **yüksek** olanları getir (`notlar.mat`)
7. Puanları dizisinde 9’dan büyük **herhangi bir** puan olanlar
8. Hem yaş hem ortalama için karşılaştırma
9. Aynı alan üzerinde birden çok karşılaştırma
10. `$eq` ve `$ne` birlikte kullanım
