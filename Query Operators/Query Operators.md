
MongoDB'de **Query Operators (Sorgu Operatörleri)**, belgeleri sorgularken koşulları belirtmek için kullanılır. SQL’deki `WHERE`, `AND`, `OR`, `<`, `=`, `LIKE` gibi operatörlerin MongoDB'deki karşılıklarıdır.

---

## 🔍 Sorgu Operatörleri Türleri

MongoDB’de sorgu operatörleri 5 ana başlık altında toplanabilir:

| Kategori                                                            | Açıklama                                   |
| ------------------------------------------------------------------- | ------------------------------------------ |
| 🔹 Karşılaştırma Operatörleri (`$gt`, `$eq` vs.)                    | Sayısal ve eşitlik koşulları               |
| 🔹 Mantıksal Operatörler (`$and`, `$or`, `$not`, `$nor`)            | Koşulları birleştirme                      |
| 🔹 Eleman Operatörleri (`$exists`, `$type`)                         | Belirli alanların olup olmadığını sorgular |
| 🔹 Dizi Operatörleri (`$in`, `$nin`, `$all`, `$size`, `$elemMatch`) | Dizi türündeki alanlara özel               |
| 🔹 Değerlendirme Operatörleri (`$expr`, `$regex`, `$mod`, `$where`) | Fonksiyonel / özel durumlar                |

---

Aşağıda her kategori için en sık kullanılan operatörleri örneklerle açıklıyorum.

---

## 🔷 1. Karşılaştırma Operatörleri

| Operatör | Anlamı        | Örnek                   |
| -------- | ------------- | ----------------------- |
| `$eq`    | eşittir       | `{ yas: { $eq: 25 } }`  |
| `$ne`    | eşit değildir | `{ yas: { $ne: 18 } }`  |
| `$gt`    | büyüktür      | `{ yas: { $gt: 18 } }`  |
| `$gte`   | büyük eşittir | `{ yas: { $gte: 18 } }` |
| `$lt`    | küçüktür      | `{ yas: { $lt: 30 } }`  |
| `$lte`   | küçük eşittir | `{ yas: { $lte: 30 } }` |

### Örnek:

```js
db.kullanicilar.find({ yas: { $gt: 20, $lt: 30 } })
```

---

## 🔷 2. Mantıksal Operatörler

| Operatör | Açıklama                     | Örnek                                                     |
| -------- | ---------------------------- | --------------------------------------------------------- |
| `$and`   | Tüm koşullar doğru olmalı    | `{ $and: [ { sehir: "Ankara" }, { yas: { $gt: 18 } } ] }` |
| `$or`    | En az bir koşul doğru olmalı | `{ $or: [ { sehir: "Ankara" }, { yas: { $lt: 18 } } ] }`  |
| `$not`   | Koşulu tersine çevirir       | `{ yas: { $not: { $gt: 18 } } }`                          |
| `$nor`   | Tüm koşullar yanlış olmalı   | `{ $nor: [ { sehir: "Ankara" }, { yas: 25 } ] }`          |

---

## 🔷 3. Eleman Operatörleri

| Operatör  | Açıklama         | Örnek                            |
| --------- | ---------------- | -------------------------------- |
| `$exists` | Alan mevcut mu?  | `{ telefon: { $exists: true } }` |
| `$type`   | Veri tipi nedir? | `{ yas: { $type: "int" } }`      |

### Örnek:

```js
db.kullanicilar.find({ email: { $exists: false } })
```

---

## 🔷 4. Dizi Operatörleri

| Operatör     | Açıklama                                   | Örnek                                                      |
| ------------ | ------------------------------------------ | ---------------------------------------------------------- |
| `$in`        | Belirtilen değerlerden biri varsa          | `{ sehir: { $in: ["Ankara", "İzmir"] } }`                  |
| `$nin`       | Belirtilen değerlerden hiçbiri yoksa       | `{ sehir: { $nin: ["Paris", "Berlin"] } }`                 |
| `$all`       | Dizide tüm elemanlar varsa                 | `{ etiketler: { $all: ["php", "backend"] } }`              |
| `$size`      | Dizi uzunluğu eşitse                       | `{ etiketler: { $size: 3 } }`                              |
| `$elemMatch` | Dizi içindeki bir obje şu kurallara uymalı | `{ yorumlar: { $elemMatch: { yildiz: 5, aktif: true } } }` |

---

## 🔷 5. Değerlendirme Operatörleri

| Operatör | Açıklama                         | Örnek                                        |
| -------- | -------------------------------- | -------------------------------------------- |
| `$regex` | Regex (düzenli ifade) eşleştirme | `{ ad: { $regex: /^A/ } }`                   |
| `$expr`  | Alanlar arası karşılaştırma      | `{ $expr: { $gt: ["$puan", "$ortalama"] } }` |
| `$mod`   | Mod işlemi (bölünebilirlik)      | `{ yas: { $mod: [5, 0] } }`                  |
| `$where` | JavaScript ile özel sorgu        | `{ $where: "this.yas + this.puan > 100" }`   |

---

## 🎯 Toplu Sorgu Örneği

```js
db.kullanicilar.find({
  $and: [
    { yas: { $gte: 20, $lte: 30 } },
    { sehir: { $in: ["Ankara", "İzmir"] } },
    { telefon: { $exists: true } }
  ]
})
```

---

## ✅ Sorgularda Performans İçin Notlar

* `$regex`, `$where`, `$not`, `$elemMatch` gibi operatörler **yavaşlatıcı olabilir**.
* Büyük verilerde sorgu performansı için **index** kullanımı önemlidir.
* `$expr`, `$mod`, `$type` gibi operatörler **CPU-intensive** olabilir.
