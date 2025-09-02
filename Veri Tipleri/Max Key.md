
👉 **MaxKey**, MongoDB’de _özel bir BSON veri türüdür_ (BSON type code: `0x7F`).  
👉 Anlamı:

> **Bir alandaki mümkün olan en büyük değeri temsil eder.**

➡ Yani MaxKey:  
✅ Tüm diğer veri türlerinden ve değerlerinden daha büyüktür.  
✅ Sıralamada her zaman en sondadır.

---

## 🌟 **Nasıl davranır?**

MongoDB sıralama ve karşılaştırmalarda şöyle bir sıra oluşturur:

```
MinKey < Null < Numbers < String < Object < Array < ... < MaxKey
```

➡ **MaxKey her zaman en büyük değeri temsil eder.**

---

## ⚡ **Nerede kullanılır?**

✅ **Sıralı tarama bitişini tanımlamak için:**  
Index taramalarında veya sorgularda “en büyük değere kadar” diyebilirsin.

✅ **Shard key aralıkları tanımlarken:**  
Bir parçanın üst sınırı MaxKey olabilir → “Bu shard, en büyük değere kadar olan tüm belgeleri tutar.”

✅ **Sınır testi, sentinel (işaretçi) değer olarak:**  
Belli bir alan için garanti en büyük değer tanımlamak istediğinde kullanılır.

---

## 📝 **Örnek Belge**

```json
{
  "puan": { "$maxKey": 1 }
}
```

➡ `puan` alanı MaxKey olarak saklanmıştır.

---

## 🎯 **Karşılaştırmalı Sıralama Örneği**

```js
db.ornek.insertMany([
  { ad: "A", puan: 100 },
  { ad: "B", puan: null },
  { ad: "C", puan: MaxKey }
])

db.ornek.find().sort({ puan: 1 })
```

✅ Çıktı sırası:

```
B (null)
A (100)
C (MaxKey)
```

➡ **MaxKey en büyük sırada çıkar.**

---

## 🌟 **SQL ile Kıyas**

SQL’de MaxKey gibi bir kavram yoktur ama örneğin sabit büyük bir değer kullanılabilir:

```sql
WHERE puan <= 999999999
```

➡ Ama bu sabit sınır MaxKey kadar evrensel değildir.

---

## 🌟 **Avantajları**

✅ Sıralamada en büyük sınır olarak net bir şekilde tanımlıdır  
✅ Shard aralıkları için üst sınır belirlemekte çok işe yarar  
✅ Sistemsel işlemler ve sınır kontrollerinde kullanışlıdır

---

## 🚩 **Dezavantajları**

❌ Uygulama düzeyinde çoğunlukla gereksizdir  
❌ Yanlış kullanılırsa gereksiz geniş aralık sorgularına sebep olabilir  
❌ Genellikle sistem ve altyapı düzeyinde anlamlıdır

---

## 🌟 **MinKey vs MaxKey**

|Özellik|MinKey|MaxKey|
|---|---|---|
|Anlamı|En küçük değer|En büyük değer|
|Sıralama konumu|En başta|En sonda|
|Kullanım|Alt sınır tanımı|Üst sınır tanımı|

---

## 🎯 **Ne zaman kullanılır?**

✅ Shard key aralıklarının üst sınırını belirlemek için  
✅ Özel sıralı tarama bitiş noktası tanımlamak için  
✅ Sistem düzeyinde sınır kontrolü gereken yerlerde

---

## 🌟 **Örnek Sorgu: MaxKey olan belgeleri bul**

```js
db.ornek.find({ puan: { $type: "maxKey" } })
```

## 🌟 **Örnek Sorgu: Bir alanda MaxKey’den küçükleri getir**

```js
db.ornek.find({ puan: { $lt: MaxKey } })
```

➡ Bütün normal belgeler döner, çünkü her şey MaxKey’den küçüktür.
