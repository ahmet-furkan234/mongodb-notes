
Birden fazla koşuldan **en az birinin doğru olması** durumunda belge eşleşir.  
SQL’deki `WHERE x OR y OR z` ifadesine denktir.

---

## 🔹 Temel Sözdizimi:

```js
db.koleksiyon.find({
  $or: [
    { koşul1 },
    { koşul2 },
    { koşul3 },
    ...
  ]
})
```

---

## 🧪 Temel Örnekler

### 🎯 1. Yaşı 18 **veya** 22 olanlar:

```js
db.ogrenciler.find({
  $or: [
    { yas: 18 },
    { yas: 22 }
  ]
})
```

---

### 🎯 2. Ortalama 80 **veya** matematik notu 90 olanlar:

```js
db.ogrenciler.find({
  $or: [
    { ortalama: 80 },
    { "notlar.mat": 90 }
  ]
})
```

---

### 🎯 3. Yaşı 20 **veya** puanlar dizisinde 10’dan büyük eleman olanlar:

```js
db.ogrenciler.find({
  $or: [
    { yas: 20 },
    { puanlar: { $gt: 10 } }
  ]
})
```

---

## 🔀 `$or` içinde `$and` kullanımı

### 🎯 Yaşı 20 veya 22 **ve** ortalaması 85 olanlar:

```js
db.ogrenciler.find({
  $or: [
    { yas: 20 },
    { yas: 22 }
  ],
  ortalama: 85
})
```

> Bu sorgu aslında şu anlama gelir:  
> `(yas=20 OR yas=22) AND ortalama=85`

---

## 🧠 `$or` ve Tekrar Eden Alanlar

`$or` içindeki koşullar aynı alanı kontrol edebilir:

```js
db.ogrenciler.find({
  $or: [
    { yas: { $lt: 18 } },
    { yas: { $gt: 30 } }
  ]
})
```

Bu sorgu, yaşı 18’den küçük **veya** 30’dan büyük olanları getirir.

---

## 📌 Dikkat:

- `$or` koşullarının hepsi **bir dizi içinde** olmalıdır
- `$or` sorgusu dışında ek alan sorguları varsa **bunlar AND gibi işlem görür**

---

## 🎯 Challenge Görevleri – `$or`

1. Yaşı 18 veya 22 olanlar
2. Ortalama 70 **veya** matematik notu 85 olanlar
3. Yaşı 20 veya puanlar dizisinde 9’dan büyük olanlar
4. Yaşı 25, 30 veya 35 olanlar
5. Ortalama 60’tan küçük **veya** matematik notu 50’den küçük olanlar
