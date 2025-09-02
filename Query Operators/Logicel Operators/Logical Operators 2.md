
## 🔷 2+ Koşul Kullanımı

### 📌 Yapı:

```js
db.koleksiyon.find({
  $and: [
    { koşul1 },
    { koşul2 },
    { koşul3 },
    ...
  ]
})
```

Aynı mantık `$or`, `$nor` için de geçerlidir.

---

## 🧪 Örnek: 3 Koşullu `$and`

### 🎯 Yaşı 20 olan, ortalaması 80’den büyük **ve** matematik notu 85’ten büyük olan öğrenciler:

```js
db.ogrenciler.find({
  $and: [
    { yas: 20 },
    { ortalama: { $gt: 80 } },
    { "notlar.mat": { $gt: 85 } }
  ]
})
```

---

## 🧪 Örnek: 3 Koşullu `$or`

### 🎯 Yaşı 17 **veya** ortalaması 55 **veya** mat notu 50 olanlar:

```js
db.ogrenciler.find({
  $or: [
    { yas: 17 },
    { ortalama: 55 },
    { "notlar.mat": 50 }
  ]
})
```

---

## 🔀 `$and` içinde `$or` (İç içe mantıksal işlemler)

### 🎯 Yaşı 20 **ve** (ortalaması 80 **veya** 90) olan öğrenciler:

```js
db.ogrenciler.find({
  $and: [
    { yas: 20 },
    {
      $or: [
        { ortalama: 80 },
        { ortalama: 90 }
      ]
    }
  ]
})
```

> 🔥 Bu tür iç içe kullanım, daha karmaşık sorgular yazmamızı sağlar.

---

## 📌 Not:

- `$and` kullanmak yerine, tek satırda `{ yas: 20, ortalama: { $gt: 80 } }` de yazılabilir. Ama 3+ koşulda **okunabilirlik** ve **dinamik oluşturma** için `$and` tercih edilir.
- `$not` sadece **tek bir alana** uygulanır ve genellikle başka bir operatörü neg'lemek için kullanılır (`$not: { $lt: 80 }` gibi).

---

## 🎯 Mini Challenge:

Yaşı 20 **ve** ortalaması 85’ten büyük **ve** puanlar dizisinde 9’dan büyük en az bir eleman olanlar.

Hazırsan önce bu örneği birlikte yazalım mı?  
Yoksa `$in`, `$all`, `$elemMatch` gibi **Array Operators** konusuna geçelim mi TkMatE?