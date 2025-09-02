
MongoDB’de en temel ve en yaygın kullanılan koleksiyon türüdür.

---

## 🧱 Nedir?

- İçinde BSON belgeleri (dokümanlar) tutan yapı.
- SQL’deki tabloya benzer ama şemasızdır (her belge farklı yapıda olabilir).
- Dinamik ve esnektir.
- Her türlü veri tipini ve yapısını depolayabilir.
- Veri ekleme, güncelleme, silme ve sorgulama yapılabilir.

---

## ⚙️ Oluşturma

MongoDB’de koleksiyon oluşturmanın iki yolu vardır:

### 1. Otomatik Oluşturma (Insert ile)

Koleksiyon **oluşturmak için explicit komut kullanmak zorunda değilsin**. İlk veri eklediğinde koleksiyon otomatik oluşur.

```js
db.normalCollection.insertOne({ name: "TkMatE", age: 30 })
```

Bu komut koleksiyon yoksa oluşturur, sonra veri ekler.

---

### 2. Explicit Oluşturma

```js
db.createCollection("normalCollection")
```

Bu komut koleksiyonu boş olarak oluşturur.

---

## 🔍 Özellikleri

|Özellik|Açıklama|
|---|---|
|Şema Yok (Schema-less)|Her belge farklı yapıda olabilir|
|Dinamik Veri|Yeni alanlar eklenebilir, yapı değişebilir|
|Index Desteği|İstenilen alanlarda index oluşturulabilir|
|CRUD İşlemleri|Veri ekleme, silme, güncelleme ve sorgulama yapılır|
|Sınırsız Boyut|Veri büyüklüğü için sınırlama yoktur (donanım limitleri var)|

---

## 📋 Örnek

```js
db.users.insertMany([
  { name: "Ahmet", age: 25, email: "ahmet@example.com" },
  { name: "Ayşe", email: "ayse@example.com", hobbies: ["reading", "swimming"] },
  { name: "Mehmet", age: 32, active: true }
])
```

- Her belge farklı alanlar içerebilir.
- Hobi listesi, bool değer vs her türlü veri tipi desteklenir.

---

## 🛠️ CRUD İşlemleri Örnekleri

### Create (Ekleme)

```js
db.users.insertOne({ name: "Ersin", age: 28 })
```

### Read (Okuma)

```js
db.users.find({ age: { $gt: 25 } })
```

### Update (Güncelleme)

```js
db.users.updateOne({ name: "Ersin" }, { $set: { active: true } })
```

### Delete (Silme)

```js
db.users.deleteOne({ name: "Ersin" })
```

---

## ⚠️ Dikkat Edilmesi Gerekenler

- **Şema olmaması** esneklik sağlar ama karmaşaya yol açabilir.  
- Bu yüzden genelde uygulama tarafında ya da MongoDB 4.0+ ile gelen **Schema Validation** özellikleri kullanılır.
- Büyük veri ve sık sorgularda performans için **index oluşturmak şarttır**.

---

## 🧠 Notlar

- Normal collection, veritabanında veri tutmanın temel yoludur.
- İhtiyacına göre indexler, validation kuralları ekleyerek yönetebilirsin.
- Capped veya TTL gibi özel koleksiyonlar ihtiyaç oldukça tercih edilir.
