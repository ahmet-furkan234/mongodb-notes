
Verilen koşulların **hiçbiri doğru değilse** belgeyi eşleştirir.  
Yani koşulların **tümü yanlış olmalı**.

---

## 🔹 Temel Sözdizimi:

```js
db.koleksiyon.find({
  $nor: [
    { koşul1 },
    { koşul2 },
    { koşul3 },
    ...
  ]
})
```

---

## 🧪 Temel Örnekler

### 🎯 1. Yaşı **20 değil** ve ortalaması **90 değil** olanlar:

```js
db.ogrenciler.find({
  $nor: [
    { yas: 20 },
    { ortalama: 90 }
  ]
})
```

> Burada yaşı 20 olan veya ortalaması 90 olan belgeler **çıkarılır**.

---

### 🎯 2. Matematik notu 80 veya 85 olanlar dışındaki öğrenciler:

```js
db.ogrenciler.find({
  $nor: [
    { "notlar.mat": 80 },
    { "notlar.mat": 85 }
  ]
})
```

---

### 🎯 3. Yaşı 18 veya 22 olanlar dışındaki öğrenciler:

```js
db.ogrenciler.find({
  $nor: [
    { yas: 18 },
    { yas: 22 }
  ]
})
```

---

## 🧠 `$nor` Operatörünün Anlamı

- `$nor`, `$or`’un **tersidir**.
- `$or` koşullarından **en az biri sağlanırsa eşleşir**.
- `$nor` koşullarından **hiçbiri sağlanmazsa eşleşir**.

---

## 📌 Dikkat:

- `$nor` tüm koşulları **bir dizi içinde** alır.
- Tek başına kullanılır, başka mantıksal operatörlerle genelde `$and` içinde kombine edilir.

---

## 🎯 Challenge Görevleri – `$nor`

1. Yaşı 20 **ne de** 22 olanlar dışındaki öğrenciler
2. Ortalama 75 **ne de** 80 olanlar dışındaki öğrenciler
3. Matematik notu 90 **ne de** 95 olanlar dışındaki öğrenciler
4. `puanlar` dizisi tam olarak `[10, 10, 9]` **ne de** `[9, 9, 9]` olanlar dışındaki öğrenciler
5. Yaşı 18 **ne de** 30 olanlar dışındaki ve ortalaması 70 üstü olanlar
