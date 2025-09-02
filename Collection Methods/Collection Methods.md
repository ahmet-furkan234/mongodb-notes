
MongoDB’de **koleksiyon (collection)**, birden fazla dokümanı barındıran yapılardır. SQL’deki tabloya benzer. Koleksiyonlar üzerinde veri eklemek, silmek, güncellemek, aramak gibi işlemler için çeşitli metodlar kullanılır.

**Collection methods**, doğrudan bir koleksiyon üzerinde işlem yapmanı sağlayan fonksiyonlardır. Bunlar Mongo Shell’de, Compass’ta ya da uygulama kodunda kullanılır.

Koleksiyon üzerinde işlem yapmak için genelde şu yapı kullanılır:

```js
db.koleksiyonAdi.metodAdi()
```

ör.:

```js
db.users.find()
```

---

## 🔑 **Koleksiyon Methodlarının Genel Amacı**

Collection metodları ile şunları yaparız:  
✅ Doküman ekleme (insert)  
✅ Doküman okuma (find, findOne)  
✅ Doküman güncelleme (update, updateOne, updateMany, replaceOne)  
✅ Doküman silme (deleteOne, deleteMany)  
✅ Koleksiyon üzerinde istatistik veya yapı bilgisi alma (stats, countDocuments, distinct)  
✅ Endeks oluşturma (createIndex)

---

## 💡 **Temel Collection Methodları Genel Başlıklar**

|Metod|Amaç|
|---|---|
|`insertOne()`|Koleksiyona tek bir doküman ekler|
|`insertMany()`|Koleksiyona birden fazla doküman ekler|
|`find()`|Şartlara uyan dokümanları listeler|
|`findOne()`|Şartlara uyan ilk dokümanı getirir|
|`updateOne()`|İlk uyan dokümanı günceller|
|`updateMany()`|Şartlara uyan tüm dokümanları günceller|
|`replaceOne()`|Bir dokümanı tamamen yenisiyle değiştirir|
|`deleteOne()`|İlk uyan dokümanı siler|
|`deleteMany()`|Şartlara uyan tüm dokümanları siler|
|`countDocuments()`|Doküman sayısını verir|
|`distinct()`|Belirli bir alan için farklı değerleri verir|
|`createIndex()`|Koleksiyon üzerinde bir alan için indeks oluşturur|
|`drop()`|Koleksiyonu siler|

---

## 📌 **Collection Methodlarının Kullanıldığı Tipik Akış**

Bir MongoDB koleksiyonu ile çalışırken:  
1️⃣ Önce bir koleksiyon seçersin (ör. `db.users`)  
2️⃣ Sonra methodu uygularsın (ör. `find`, `insertOne`, `deleteMany`)

Örneğin:

```js
db.users.insertOne({ ad: "Ali", yas: 25 });
db.users.find({ yas: { $gte: 18 } });
db.users.updateOne({ ad: "Ali" }, { $set: { yas: 26 } });
db.users.deleteOne({ ad: "Ali" });
```

---

## ⚠️ **Dikkat Edilecek Noktalar**

- MongoDB metodları genelde **non-relational** veri mantığıyla çalışır. Yani SQL’deki gibi tablolar arası join bekleme.
- Filtreleme için çoğu metodda bir **query (sorgu nesnesi)** ve bir **update nesnesi** ya da **opsiyonlar** gönderilir.
- Büyük koleksiyonlarda performans için `index` ve `projection` (hangi alanları getireceğini belirleme) kullanılır.