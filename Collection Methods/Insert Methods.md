
MongoDB’de veri eklemek için kullanılan methodlar:

|Yöntem|Açıklama|SQL Karşılığı|
|---|---|---|
|`insertOne()`|Tek bir doküman ekler.|`INSERT INTO tablo ... VALUES (...)`|
|`insertMany()`|Çok sayıda dokümanı aynı anda ekler.|`INSERT INTO tablo ... VALUES (...), (...), ...`|
|`insert()`|Eskiden hem tek hem çoklu ekleme için kullanılırdı, ama artık `insertOne` ve `insertMany` kullanılır.|Eski kullanım|

MongoDB’de doküman eklenirken bir **ObjectId** otomatik olarak atanır ve `_id` alanı oluşur (SQL’deki `PRIMARY KEY` gibi düşünebilirsin).

---

## 🚀 **insertOne()**

### ➡ Tek bir doküman ekler.

### 📌 Kullanım:

```js
db.users.insertOne({
  ad: "Ali",
  yas: 25,
  sehir: "Ankara"
})
```

💡 Bu işlem sonrası eklenen doküman:

```json
{
  "_id": ObjectId("..."),
  "ad": "Ali",
  "yas": 25,
  "sehir": "Ankara"
}
```

### 🔑 SQL Karşılığı:

```sql
INSERT INTO users (ad, yas, sehir) VALUES ('Ali', 25, 'Ankara');
```

### ⚠ Dikkat:

- `_id` alanını sen vermezsen MongoDB otomatik atar.
- `_id`’yi sen verirsen, aynı `id` ile tekrar eklemeye çalışırsan **duplicate key error** alırsın.

---

## 🚀 **insertMany()**

### ➡ Birden fazla dokümanı aynı anda ekler.

### 📌 Kullanım:

```js
db.users.insertMany([
  { ad: "Ayşe", yas: 22, sehir: "İzmir" },
  { ad: "Mehmet", yas: 30, sehir: "İstanbul" },
  { ad: "Zeynep", yas: 28, sehir: "Bursa" }
])
```

💡 Her dokümana ayrı bir `_id` atanır.

### 🔑 SQL Karşılığı:

```sql
INSERT INTO users (ad, yas, sehir)
VALUES 
  ('Ayşe', 22, 'İzmir'),
  ('Mehmet', 30, 'İstanbul'),
  ('Zeynep', 28, 'Bursa');
```

### ⚠ Dikkat:

- Eğer tek bir dokümanda hata olursa (ör. `_id` çakışması), diğerlerinin eklenip eklenmeyeceğini **ordered** seçeneği belirler:

```js
db.users.insertMany([...], { ordered: false })
```

👉 `ordered: false` → Hatalı kayıt atlanır, diğerleri eklenir.

---

## 🚀 **insert() (Deprecated - eski yöntem)**

Eskiden hem tek hem çoklu ekleme için kullanılırdı:

```js
db.users.insert({ ad: "Ali", yas: 25 })
db.users.insert([
  { ad: "Ahmet", yas: 35 },
  { ad: "Fatma", yas: 29 }
])
```

💡 **Artık önerilmiyor!** `insertOne` ve `insertMany` kullanmak doğru yol.

---

## 🎯 **SQL ve MongoDB Insert Karşılaştırması**

|Özellik|SQL INSERT|MongoDB INSERT|
|---|---|---|
|Alanlar|Önceden tablo şemasında tanımlı|Dokümanda serbest yapı (schema-less)|
|Otomatik ID|AUTO_INCREMENT / GUID|ObjectId (otomatik atanır)|
|Veri türü kontrolü|Sıkı kontrol (ör. INT, VARCHAR)|Esnek (farklı tipler olabilir)|
|Çoklu insert|`INSERT ... VALUES (...), (...), ...`|`insertMany()` ile array halinde|
|Zorunlu alanlar|Tablo şemasına göre zorunludur|MongoDB tarafında zorunlu alan yok (istenirse validation ile yapılır)|

---

## ⚡ **Detaylı Örnekler**

### ✅ **Custom _id ile ekleme**

```js
db.users.insertOne({
  _id: "user_001",
  ad: "Veli",
  yas: 40
})
```

➡ `_id` artık ObjectId değil, senin verdiğin string oldu.

---

### ✅ **insertMany + ordered false**

```js
db.users.insertMany([
  { _id: 1, ad: "Ahmet" },
  { _id: 2, ad: "Fatma" },
  { _id: 1, ad: "Duplicate Ahmet" } // Çakışma
], { ordered: false })
```

👉 İlk iki doküman eklenir, üçüncü atlanır.

---

### ✅ **insertMany + nested document**

```js
db.products.insertMany([
  {
    ad: "Laptop",
    fiyat: 15000,
    stok: { adet: 20, depo: "A1" }
  },
  {
    ad: "Mouse",
    fiyat: 300,
    stok: { adet: 100, depo: "B2" }
  }
])
```

➡ Dokümanlar iç içe (nested) yapılarla eklenebilir.  
👉 SQL’de bunu yapmak için başka bir tablo + ilişki gerekirdi!

---

## 🔍 **insertOne + Validation Örneği**

MongoDB’de bir koleksiyonda validation tanımladıysan:

```js
db.createCollection("students", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["ad", "yas"],
      properties: {
        ad: { bsonType: "string" },
        yas: { bsonType: "int", minimum: 0 }
      }
    }
  }
})
```

Sonra:

```js
db.students.insertOne({ ad: "Cem", yas: 21 }) // ✅ başarılı
db.students.insertOne({ ad: "Cem" }) // ❌ yas eksik, hata verir
```

➡ SQL’de `NOT NULL` ve `CHECK` constraint’lere benzer.

---

## ⚡ **Performans ve İpuçları**

✅ Çoklu veri eklerken **insertMany** her zaman daha performanslıdır.  
✅ Tek tek döngüyle `insertOne` çağırmak yerine array ile `insertMany` kullan.  
✅ Büyük verilerde **bulk write** (toplu yazma) yöntemlerine geç.  
✅ `_id` alanını verirken dikkatli ol, çakışma olursa ekleme başarısız olur.

---

## 🎁 **Bonus: insertMany için Bulk SQL ve MongoDB Karşılaştırması**

### SQL:

```sql
INSERT INTO orders (id, urun_adi, miktar)
VALUES 
  (1, 'Kalem', 10),
  (2, 'Silgi', 5),
  (3, 'Defter', 7);
```

### MongoDB:

```js
db.orders.insertMany([
  { _id: 1, urun_adi: "Kalem", miktar: 10 },
  { _id: 2, urun_adi: "Silgi", miktar: 5 },
  { _id: 3, urun_adi: "Defter", miktar: 7 }
])
```

✅ MongoDB’de alanlar serbest, her doküman farklı yapıda olabilir.  
✅ SQL’de tüm satırlar aynı şemaya uymak zorunda.

---

## 💬 **Özet**

|MongoDB Method|Kullanım Amacı|
|---|---|
|`insertOne`|Tek bir doküman eklemek|
|`insertMany`|Çok sayıda dokümanı hızlıca eklemek|
|`insert`|Eskiden hem tek hem çoklu ekleme için kullanılırdı (deprecated)|
