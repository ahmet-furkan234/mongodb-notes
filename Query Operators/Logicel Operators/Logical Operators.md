
| Operatör | Açıklama                                      |
| -------- | --------------------------------------------- |
| `$and`   | Tüm koşullar **doğruysa** eşleşir (ve)        |
| `$or`    | En az bir koşul doğruysa eşleşir (veya)       |
| `$not`   | Koşul **yanlışsa** eşleşir (değil)            |
| `$nor`   | **Hiçbiri doğru değilse** eşleşir (ne ... ne) |

---

## 🔹 1. `$and` – **VE** Operatörü

### 📌 Söz dizimi:

```js
{ $and: [ { koşul1 }, { koşul2 } ] }
```

### 🧪 Örnek: Yaşı 20 **ve** ortalaması 80 olanlar

```js
db.ogrenciler.find({
  $and: [
    { yas: 20 },
    { ortalama: 80 }
  ]
})
```

> Not: `{ yas: 20, ortalama: 80 }` yazmak da aynı anlama gelir. `$and` özellikle dinamik sorgularda tercih edilir.

---

## 🔹 2. `$or` – **VEYA** Operatörü

### 📌 Söz dizimi:

```js
{ $or: [ { koşul1 }, { koşul2 } ] }
```

### 🧪 Örnek: Yaşı 18 **veya** ortalaması 90 olanlar

```js
db.ogrenciler.find({
  $or: [
    { yas: 18 },
    { ortalama: 90 }
  ]
})
```

> En az bir koşul sağlanıyorsa belge eşleşir.

---

## 🔹 3. `$not` – **DEĞİL** Operatörü

### 📌 Söz dizimi:

```js
{ alan: { $not: { koşul } } }
```

### 🧪 Örnek: Ortalaması **80 olmayanlar**

```js
db.ogrenciler.find({
  ortalama: { $not: { $eq: 80 } }
})
```

> Genelde `$not` doğrudan bir değer değil, **başka bir operatörü** reddetmek için kullanılır.

---

## 🔹 4. `$nor` – **NE... NE DE** Operatörü

### 📌 Söz dizimi:

```js
{ $nor: [ { koşul1 }, { koşul2 } ] }
```

### 🧪 Örnek: Yaşı 20 **değil**, ortalaması da 90 **değil** olanlar:

```js
db.ogrenciler.find({
  $nor: [
    { yas: 20 },
    { ortalama: 90 }
  ]
})
```

> İki koşul da sağlanmıyorsa belge eşleşir.

---

## 🧪 Challenge Görevleri – Logical Operators

1. Yaşı 20 **ve** ortalaması 75 **üstü** olanlar
2. Yaşı 18 **veya** 30 olanlar
3. Matematik notu **80’den küçük olmayanlar** (`$not`)
4. Yaşı ne 20 **ne de** 22 olanlar
5. Ortalama **80 olmayanlar** VE mat notu **95’ten küçük olanlar**
