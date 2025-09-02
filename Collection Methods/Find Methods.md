
MongoDB'de:

- `find()` → Koleksiyondan **birden fazla doküman** getirir (şartlara uyan tüm dokümanları döner).
- `findOne()` → Koleksiyondan **tek bir doküman** getirir (şartlara uyan ilk dokümanı döner).

👉 SQL karşılığı:

|MongoDB|SQL|
|---|---|
|`find()`|`SELECT * FROM tablo WHERE ...`|
|`findOne()`|`SELECT * FROM tablo WHERE ... LIMIT 1`|

---

## 🔑 **Sözdizimi**

```js
db.koleksiyon.find(query, projection)
db.koleksiyon.findOne(query, projection)
```

|Parametre|Açıklama|
|---|---|
|`query`|Filtreleme şartları (WHERE karşılığı)|
|`projection`|Hangi alanların getirileceği (SELECT sütunları karşılığı)|

---

## 🚀 **find() Örnekleri**

---

### 1️⃣ **Tüm dokümanları getir**

```js
db.users.find()
```

SQL karşılığı:

```sql
SELECT * FROM users;
```

---

### 2️⃣ **Şart ile dokümanları getir**

```js
db.users.find({ sehir: "Ankara" })
```

SQL:

```sql
SELECT * FROM users WHERE sehir = 'Ankara';
```

---

### 3️⃣ **Karşılaştırma operatorleri**

```js
db.users.find({ yas: { $gte: 18 } })
```

SQL:

```sql
SELECT * FROM users WHERE yas >= 18;
```

Diğer karşılaştırmalar:

|MongoDB|SQL|
|---|---|
|`$eq`|`=`|
|`$ne`|`!=`|
|`$gt`|`>`|
|`$gte`|`>=`|
|`$lt`|`<`|
|`$lte`|`<=`|

Örnek:

```js
db.users.find({ yas: { $gt: 18, $lt: 30 } })
```

SQL:

```sql
SELECT * FROM users WHERE yas > 18 AND yas < 30;
```

---

### 4️⃣ **Mantıksal operatorler**

```js
db.users.find({ $or: [ { sehir: "Ankara" }, { sehir: "İstanbul" } ] })
```

SQL:

```sql
SELECT * FROM users WHERE sehir = 'Ankara' OR sehir = 'İstanbul';
```

```js
db.users.find({ $and: [ { yas: { $gte: 18 } }, { sehir: "Ankara" } ] })
```

SQL:

```sql
SELECT * FROM users WHERE yas >= 18 AND sehir = 'Ankara';
```

---

### 5️⃣ **Projection (Alan seçimi)**

```js
db.users.find(
  { sehir: "Ankara" },
  { ad: 1, yas: 1, _id: 0 }
)
```

➡ Sadece `ad` ve `yas` alanlarını getir, `_id` olmasın.

SQL:

```sql
SELECT ad, yas FROM users WHERE sehir = 'Ankara';
```

---

### 6️⃣ **Sıralama (sort)**

```js
db.users.find().sort({ yas: -1 })
```

➡ `yas` azalan sırada  
SQL:

```sql
SELECT * FROM users ORDER BY yas DESC;
```

---

### 7️⃣ **Limit + Skip**

```js
db.users.find().skip(10).limit(5)
```

➡ 10 dokümanı atla, sonraki 5 dokümanı getir

SQL:

```sql
SELECT * FROM users LIMIT 5 OFFSET 10;
```

---

## 🚀 **findOne() Örnekleri**

### 1️⃣ **İlk uyan dokümanı getir**

```js
db.users.findOne({ sehir: "Ankara" })
```

SQL:

```sql
SELECT * FROM users WHERE sehir = 'Ankara' LIMIT 1;
```

---

### 2️⃣ **Projection ile**

```js
db.users.findOne(
  { yas: { $gte: 18 } },
  { ad: 1, sehir: 1, _id: 0 }
)
```

SQL:

```sql
SELECT ad, sehir FROM users WHERE yas >= 18 LIMIT 1;
```

---

## ⚡ **Special Query Operatörleri**

|Operatör|Açıklama|Örnek|
|---|---|---|
|`$in`|Belirli bir değer listesinde arar|`{ sehir: { $in: ["Ankara", "İstanbul"] } }`|
|`$nin`|Belirli değerler dışında arar|`{ sehir: { $nin: ["Ankara", "İstanbul"] } }`|
|`$exists`|Alanın var olup olmadığını kontrol eder|`{ telefon: { $exists: true } }`|
|`$regex`|RegEx ile eşleşme|`{ ad: { $regex: "^A" } }` (A ile başlayan)|

---

## 🎯 **SQL ile Karşılaştırma Özeti**

| MongoDB              | SQL                         |
| -------------------- | --------------------------- |
| `find()`             | `SELECT * FROM ...`         |
| `findOne()`          | `SELECT * FROM ... LIMIT 1` |
| `projection`         | `SELECT kolonlar`           |
| `sort()`             | `ORDER BY`                  |
| `skip()` + `limit()` | `LIMIT ... OFFSET ...`      |
| `$in`                | `IN (...)`                  |
| `$or`, `$and`        | `OR`, `AND`                 |
| `$regex`             | `LIKE`, `REGEXP`            |

---

## ⚠ **Dikkat Edilecekler**

✅ `find()` her zaman bir cursor döner (büyük veri setlerini tek seferde çekmez, parça parça okur).  
✅ `findOne()` dökümanı doğrudan getirir, cursor döndürmez.  
✅ Projection'da 1 olanlar getirilecek alanları gösterir, ama `1` ve `0` karışık kullanılamaz (sadece `_id` hariç).

---

## 📝 **Tam Senaryo Örneği**

```js
db.users.find(
  { sehir: { $in: ["Ankara", "İstanbul"] }, yas: { $gte: 18 } },
  { ad: 1, yas: 1, sehir: 1, _id: 0 }
).sort({ yas: -1 }).skip(5).limit(10)
```

SQL:

```sql
SELECT ad, yas, sehir 
FROM users 
WHERE sehir IN ('Ankara', 'İstanbul') AND yas >= 18
ORDER BY yas DESC
LIMIT 10 OFFSET 5;
```
