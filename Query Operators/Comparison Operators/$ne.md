
Belirtilen alanın, verilen değere **eşit olmamasını** kontrol eder.  
SQL'deki `!=` operatörüne denk gelir.

---

## 🔹 Temel Sözdizimi

```js
{ alanAdi: { $ne: deger } }
```

---

## 🧪 Temel Örnekler

### 🎯 1. Yaşı **20 olmayan** öğrenciler:

```js
db.ogrenciler.find({ yas: { $ne: 20 } })
```

---

### 🎯 2. Ortalama puanı **80 olmayan** öğrenciler:

```js
db.ogrenciler.find({ ortalama: { $ne: 80 } })
```

---

### 🎯 3. Matematik notu **85 olmayanlar**:

```js
db.ogrenciler.find({ "notlar.mat": { $ne: 85 } })
```

---

### 🎯 4. `puanlar` dizisi **tam olarak** `[10, 10, 9]` olmayanlar:

```js
db.ogrenciler.find({ puanlar: { $ne: [10, 10, 9] } })
```

> 🔥 Bu **dizi karşılaştırması** yapar — birebir aynı olmayanları getirir.

---

## 🔎 Dikkat! `$ne` Sadece **eşitliği reddeder**, başka bir kontrol yapmaz:

### ❌ Örnek: Dizi içinde 10 olmayanları **sanmak**

```js
db.ogrenciler.find({ puanlar: { $ne: 10 } }) // Beklendiği gibi çalışmaz
```

Bu sorgu, `puanlar` tamamen `10` **değilse** eşleşir.  
Yani içinde `10` olan ama dizi `[9, 10, 8]` gibi olan belgeler **eşleşir**, çünkü dizi tümüyle `10` değil.

> ✅ Bunun yerine `$not` veya `$elemMatch` ile çözüm kullanılır.

---

## 🧩 `$ne` ile Birlikte Diğer Koşullar

### 🎯 5. Yaşı 20 **değil** ve ortalaması 75 **üstü** olanlar:

```js
db.ogrenciler.find({
  yas: { $ne: 20 },
  ortalama: { $gt: 75 }
})
```

---

## 🧠 `$ne` ile `null` Kontrolü

```js
db.ogrenciler.find({ ortalama: { $ne: null } })
```

Bu, **ortalama alanı olanları** getirir (ama dikkat: `exists: false` farkı da var).

---

## 🧪 Challenge Görevleri – `$ne`

1. Yaşı 20 **olmayan** öğrenciler
2. Ortalama puanı 90 **dışında** olanlar
3. Matematik notu 100 **olmayanlar**
4. `puanlar` dizisi tam olarak `[9, 10, 10]` **olmayan** öğrenciler
5. Yaşı 22 **değil** ve ortalaması 85 **olanlar**

---

## 📌 Kapanış – Comparison Operator'ları Tam Listesi

|Operatör|Görev|
|---|---|
|`$eq`|Eşittir|
|`$ne`|Eşit değildir|
|`$gt`|Büyüktür|
|`$gte`|Büyük eşit|
|`$lt`|Küçüktür|
|`$lte`|Küçük eşit|
