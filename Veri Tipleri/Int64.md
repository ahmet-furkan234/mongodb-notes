
👉 **Int64**, MongoDB BSON formatında _64 bit signed integer_ (long integer) değerleri tutmak için kullanılır (type code: `0x12`).  
👉 `-9,223,372,036,854,775,808` ile `9,223,372,036,854,775,807` aralığındaki tam sayıları depolar.

➡ Kısaca: **çok büyük tam sayılar için kullanılır.**

---

## 🌟 **Neden gerekli?**

✅ JavaScript Number türü (IEEE 754 double) tam olarak 53 bit hassasiyet sağlar.  
✅ 53 bitten büyük tam sayılar double ile tutulursa hassasiyet kaybı olur.  
✅ Bu yüzden MongoDB Int64 (Long) kullanarak büyük sayıları doğru biçimde saklar.

---

## 📝 **Örnek Belge**

```json
{
  "_id": 1,
  "bakiye": NumberLong("9223372036854775807")
}
```

👉 `bakiye` alanı maksimum Int64 değeriyle tutulmuş.

---

## ⚡ **MongoDB'de Long kullanımı**

### Long değer atama

```js
db.hesaplar.insertOne({
  ad: "Ahmet",
  bakiye: NumberLong("1234567890123456789")
})

```

### Long değer sorgulama

```js
db.hesaplar.find({ bakiye: NumberLong("1234567890123456789") })
```

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Int64 (Long)|SQL Karşılığı|
|---|---|---|
|Tip|64 bit signed integer|BIGINT (64 bit signed integer)|
|Aralık|±9 kentilyon|±9 kentilyon|
|Kullanım|Büyük tam sayılar|Büyük tam sayılar|

➡ SQL:

```sql
CREATE TABLE hesaplar (
  id BIGINT PRIMARY KEY,
  bakiye BIGINT
);
```

---

## 🌟 **Avantajları**

✅ Çok büyük tam sayıları hassasiyet kaybı olmadan tutar  
✅ Double yerine tam sayı tutmak daha doğru hesaplama sağlar  
✅ Sayısal karşılaştırma ve hesaplama işlemleri sağlıklıdır

---

## 🚩 **Dezavantajları**

❌ MongoDB shell ve bazı driver'larda dikkat ister: NumberLong ile açıkça tanımlamazsan, büyük sayılar double olarak yanlış kaydedilebilir  
❌ Int32 ile karışabilir, özellikle JSON çıktılarını işlerken dikkat gerekir  
❌ MongoDB shell’de doğrudan büyük sayılar yazarken NumberLong kullanmayı unutursan beklenmedik hatalar olabilir

---

## ⚠ **Dikkat Edilecek Noktalar**

✅ Küçük tam sayılar için Int32 yeterlidir, Int64 gereksiz yer kaplar  
✅ Uygulama ve veri taşıma katmanlarında (ör. JSON serialize) Int64 desteklenmesine dikkat et

---

## 🎯 **Ne zaman kullanılır?**

✅ Çok büyük tam sayılar (ör. işlem ID'leri, hesap bakiyeleri, sayaçlar) gerektiğinde  
✅ Global unique ID'ler, timestamp bazlı ID'ler için