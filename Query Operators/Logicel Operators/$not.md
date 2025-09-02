
Bir koşulun **tersini** almak için kullanılır.  
Yani, belirtilen koşulun **sağlanmadığı** belgeleri getirir.

---

## 🔹 Temel Sözdizimi:

```js
{ alanAdi: { $not: { koşul } } }
```

> Not: `$not` genellikle başka bir operatör ile birlikte kullanılır (örneğin `$gt`, `$eq`).

---

## 🧪 Temel Örnekler

### 🎯 1. Ortalaması **80 olmayanlar**:

```js
db.ogrenciler.find({
  ortalama: { $not: { $eq: 80 } }
})
```

---

### 🎯 2. Yaşı 20’den **küçük olmayanlar** (yani 20 ve üzeri olanlar):

```js
db.ogrenciler.find({
  yas: { $not: { $lt: 20 } }
})
```

---

### 🎯 3. Matematik notu 90’dan **büyük olmayanlar**:

```js
db.ogrenciler.find({
  "notlar.mat": { $not: { $gt: 90 } }
})
```

---

### 🎯 4. `puanlar` dizisinde 10’dan büyük bir eleman **olmayanlar**:

```js
db.ogrenciler.find({
  puanlar: { $not: { $gt: 10 } }
})
```

> Bu, dizideki tüm elemanların 10 veya daha küçük olduğu anlamına gelir.

---

## 📌 Dikkat Edilecek Noktalar

- `$not` **doğrudan değerlerle kullanılmaz**, mutlaka içinde başka bir operatör olmalı.
- `$not` sadece belirtilen alanı kontrol eder; karmaşık mantıksal ifadelerde `$and` veya `$or` ile kombine edilir.
- `$not` sorgusu **performans açısından** bazen daha yavaştır.

---

## 🧩 `$not` ile Kombinasyon Örneği

### 🎯 Yaşı 20’den büyük **ve** ortalaması 70’den küçük **olmayanlar**:

```js
db.ogrenciler.find({
  yas: { $gt: 20 },
  ortalama: { $not: { $lt: 70 } }
})
```

---

## 🎯 Challenge Görevleri – `$not`

1. Yaşı 18’den küçük **olmayanlar**
2. Ortalama 75 **olmayanlar**
3. Matematik notu 85’ten yüksek **olmayanlar**
4. `puanlar` dizisinde 8’den büyük eleman **olmayanlar**
5. Yaşı 22 **olmayanlar** ve ortalaması 80 veya daha fazla olanlar
