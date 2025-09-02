
## 📌 **Tanım**

MongoDB’de **String**, bir belge alanında metin bilgisini tutmak için kullanılan temel veri türlerinden biridir.  
➡ BSON formatında **UTF-8 kodlu** olarak saklanır.  
➡ MongoDB String’leri Unicode uyumludur, yani her dildeki karakterleri destekler.

---

## 🌟 **Özellikler**

✅ BSON formatında UTF-8 kodlanır → çok dilli veri saklamak mümkündür.  
✅ Sorgularda string’e göre filtreleme ve sıralama yapılabilir.  
✅ String’ler üzerinde **index** oluşturulabilir (ör. ad alanında index).  
✅ String tipindeki alanlarda **regex** (desen arama) desteği vardır.

---

## 📝 **Örnek Belge**

```json
{
  "ad": "Ahmet",
  "soyad": "Yılmaz",
  "meslek": "Mühendis"
}
```

👉 `ad`, `soyad`, `meslek` alanlarının hepsi String türünde.

---

## ⚡ **String Üzerinde Yapılabilecek İşlemler**

### 1️⃣ **Doğrudan eşleşme**

```js
db.kisiler.find({ "ad": "Ahmet" })
```

➡ `ad` alanı tam olarak "Ahmet" olan belgeleri getirir.

---

### 2️⃣ **Regex ile arama**

```js
db.kisiler.find({ "ad": { $regex: "^A" } })
```

➡ `ad` alanı A harfi ile başlayanları getirir.

---

### 3️⃣ **Indexleme**

String alan üzerinde index kurabilirsin:

```js
db.kisiler.createIndex({ "ad": 1 })
```

➡ ad alanına göre sorgular hızlanır.

---

## ⚠️ **Dikkat Edilecek Noktalar**

- MongoDB String’ler esnek olduğu için alanlar her belgede farklı olabilir (bir belgede `ad` alanı String, bir diğerinde olmayabilir).
- **Büyük küçük harf duyarlılığı:** Varsayılan olarak MongoDB string karşılaştırmaları büyük küçük harf duyarlıdır. (ör. `"Ahmet"` ≠ `"ahmet"`)
- **Regex sorguları index kullanmayabilir** (bazı durumlarda performans düşebilir).

---

## 🎯 **Kapanış**

👉 MongoDB’de **String** veri tipi JSON’daki string’in birebir karşılığıdır ama UTF-8 uyumluluğu ve index + regex desteği sayesinde sorgularda güçlüdür.  
👉 String veri türü MongoDB’de çok kullanılır: ad, soyad, açıklama, e-posta gibi alanlar hep String tipindedir.