
👉 **Int32**, MongoDB BSON formatında _32 bit signed integer_ değerleri tutmak için kullanılır (type code: `0x10`).  
👉 `-2,147,483,648` ile `2,147,483,647` aralığındaki tam sayıları depolar.

➡ Yani **küçük ve orta büyüklükte tam sayılar için uygundur.**

---

## 🌟 **Özellikler**

✅ 32 bit tam sayı  
✅ Daha az bellek tüketir (Int64'e göre)  
✅ Sayısal karşılaştırma ve matematik işlemler için idealdir

---

## 📝 **Örnek Belge**

```json
{
  "_id": 1,
  "yas": 35,
  "puan": NumberInt(1500)
}
```

👉 Burada `puan` alanı Int32 tipindedir.

---

## ⚡ **Int32 Kullanımı**

### Int32 ile belge ekleme

```js
db.kullanicilar.insertOne({
  ad: "Ahmet",
  yas: NumberInt(28)
})
```

➡ `yas` alanı Int32 olarak tutulur.

### Int32 ile sorgulama

```js
db.kullanicilar.find({ yas: NumberInt(28) })
```

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Int32|SQL Karşılığı|
|---|---|---|
|Tip|32 bit signed integer|INTEGER / INT|
|Aralık|±2 milyar|±2 milyar|
|Kullanım|Küçük-orta tam sayılar|Aynı (INT / INTEGER sütunlar)|

➡ SQL:

```sql
CREATE TABLE kullanicilar (
  id INT PRIMARY KEY,
  yas INT
);
```

---

## 🌟 **Avantajları**

✅ Bellek açısından verimli (Int64'e göre %50 daha küçük)  
✅ Normal tam sayılar için yeterli  
✅ Performans olarak hızlıdır

---

## 🚩 **Dezavantajları**

❌ Büyük sayılar için yetersiz kalır (2 milyar sınırına dikkat!)  
❌ Int64 ve Double ile karışık kullanılınca tip uyumsuzluğu hatalarına sebep olabilir  
❌ JSON serializasyonunda NumberInt bilgisi kaybolabilir (ör. 28 sadece bir sayı gibi görünebilir)

---

## ⚠ **Dikkat Edilecek Noktalar**

✅ Eğer sayılar Int32 sınırını geçiyorsa mutlaka Int64 (NumberLong) kullan.  
✅ Aggregation ve karşılaştırmalarda tip uyumuna dikkat et (ör. Int32 + Double işleminde Double döner).

---

## 🎯 **Ne zaman kullanılır?**

✅ Yaş, puan, sayaç, sayfa numarası gibi küçük ve orta boyutlu sayılar  
✅ Bellek ve performans kritik olduğunda (ör. büyük koleksiyonlarda küçük sayılar)
