
### 🔹 **1️⃣ INSERT — Veri eklemek**

👉 Tek bir belge eklemek:

```JS
db.koleksiyonAdi.insertOne({ ad: "Ahmet", yas: 21 })
```

👉 Birden fazla belge eklemek:


```JS
db.koleksiyonAdi.insertMany([   { ad: "Mehmet", yas: 25 },   { ad: "Ayşe", yas: 22 } ])
```

💡 _`insertOne` ve `insertMany` belgeleri koleksiyona ekler._

---

### 🔹 **2️⃣ FIND — Veri sorgulamak**

👉 Tüm belgeleri listelemek:

```JS
db.koleksiyonAdi.find()
```

> Dönen sonuç JSON benzeri bir yapı olur.

👉 Daha güzel görünmesi için:

```JS
db.koleksiyonAdi.find().pretty()
```

👉 Filtreleme:

```js
db.koleksiyonAdi.find({ ad: "Ahmet" })
```

👉 Belirli alanları getirmek (projection):

```js
db.koleksiyonAdi.find({ yas: { $gt: 20 } }, { ad: 1, _id: 0 })
```

> 20’den büyük yaştakilerin sadece adlarını getir (_id olmadan).

---

### 🔹 **3️⃣ UPDATE — Veriyi güncellemek**

👉 Tek belgeyi güncelle:


```js
db.koleksiyonAdi.updateOne(   { ad: "Ahmet" },               
// arama kriteri   { $set: { yas: 22 } }          // yeni değer )
```

👉 Çok belgeyi güncelle:

```js
db.koleksiyonAdi.updateMany(   
	{ yas: { $lt: 25 } },   
	{ $set: { aktif: true } } 
)
```

👉 Belge yoksa ekle (upsert):

```js
db.koleksiyonAdi.updateOne(   
	{ ad: "Ali" },   
	{ $set: { yas: 30 } },   
	{ upsert: true } 
)
```

---

### 🔹 **4️⃣ DELETE — Belge silmek**

👉 Tek belgeyi sil:

```js
db.koleksiyonAdi.deleteOne({ ad: "Ahmet" })
```

👉 Çok belgeyi sil:


```js
db.koleksiyonAdi.deleteMany({ yas: { $lt: 25 } })
```

---

### 🔹 **5️⃣ SHOW / USE — DB ve koleksiyonları görmek**

👉 Veritabanlarını listele:

```js
show dbs
```

👉 Veritabanını seç:

```js
use okul
```

👉 Koleksiyonları listele:

```js
show collections
```

---

## 🌟 **Hızlı Demo Akışı**

```js
use deneme
db.ogrenciler.insertOne({ ad: "Ahmet", yas: 21 })
db.ogrenciler.insertMany([{ ad: "Ayşe", yas: 22 }, { ad: "Ali", yas: 23 }])
db.ogrenciler.find().pretty()
db.ogrenciler.updateOne({ ad: "Ahmet" }, { $set: { yas: 25 } })
db.ogrenciler.deleteOne({ ad: "Ali" })
```


---

## 📝 **Notlar**

✅ `updateOne`, `updateMany`, `deleteOne`, `deleteMany` gibi komutlarda hep ilk parametre **filtre**, ikinci parametre **işlem**dir.  
✅ MongoDB’de bir tabloya denk gelen şey **koleksiyon (collection)**’dur.