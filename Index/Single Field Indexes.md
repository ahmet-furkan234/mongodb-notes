
## 🧠 Nedir Single Field Index?

Single Field Index, yalnızca **tek bir alana** göre oluşturulmuş index’tir.

```js
db.collection.createIndex({ alanAdi: 1 })   // Artan sıralama
db.collection.createIndex({ alanAdi: -1 })  // Azalan sıralama
```

- **Sıralama (`1` ya da `-1`)** sadece `sort()` işlemlerinde etkilidir.
- Sorgunun performansını artırır.
- Aramalarda, sıralamada ve filtrelemede MongoDB’ye büyük hız kazandırır.

---

## ✅ Örnek 1: Basit Kullanım

### Belgeler:

```js
{
  _id: ObjectId("..."),
  username: "tkmate",
  email: "tkmate@example.com",
  createdAt: ISODate("2025-07-12")
}
```

### Index Oluştur:

```js
db.users.createIndex({ email: 1 })
```

### Sorgu:

```js
db.users.find({ email: "tkmate@example.com" })
```

➡️ Artık MongoDB tüm koleksiyonu taramaz, **doğrudan email index'inden** bulur.

---

## 🧪 `explain()` ile Index Kullanımını Doğrula

```js
db.users.find({ email: "tkmate@example.com" }).explain("executionStats")
```

Sonuçta şunu görmelisin:

```json
"stage": "IXSCAN"
```

---

## ✅ Örnek 2: `sort()` ile Kullanım

```js
db.users.createIndex({ createdAt: -1 })

db.users.find().sort({ createdAt: -1 }).limit(10)
```

➡️ En son eklenen 10 kullanıcıyı hızlıca getirir.

> 🔥 Eğer `sort()` yapacaksan, doğru yönde (1 ya da -1) index oluşturmalısın.

---

## ✅ Örnek 3: `range` (aralık) sorgularında

```js
db.products.createIndex({ price: 1 })

db.products.find({ price: { $gte: 100, $lte: 500 } })
```

➡️ Fiyat aralığında arama yapılacaksa **kesinlikle index olmalı.**

---

## ⚠️ Dikkat: Index sadece şu durumlarda kullanılır

|Sorgu türü|Index Kullanılır mı?|
|---|---|
|`{ field: value }`|✅ Evet|
|`{ field: { $gt, $lt } }`|✅ Evet|
|`sort({ field: 1 })`|✅ Evet|
|`project({ field: 1 })`|❌ Hayır|
|`{ otherField: value }`|❌ Hayır|

---

## 🧹 Index Silme

```js
db.users.dropIndex("email_1")
```

📌 Index adını öğrenmek için:

```js
db.users.getIndexes()
```

---

## 📦 Örnek Senaryo: Kullanıcı sistemi

|Alan|Index Gerekir mi?|Neden|
|---|---|---|
|`email`|✅|Sık arama yapılıyor|
|`username`|✅|Uniqueness gerekir|
|`createdAt`|✅|Sıralama yapılacak|
|`bio`|❌|Genelde aranmaz|

---

## 💡 İpucu: Unique Index = Hem index hem de doğrulama sağlar

```js
db.users.createIndex({ username: 1 }, { unique: true })
```

➡️ Aynı `username`'i iki kez eklemeye çalışırsan hata alırsın.

---

## 🔚 Özet

|Özellik|Açıklama|
|---|---|
|Tek alana kurulur|Sadece bir field indekslenir|
|Hız kazandırır|Arama, sıralama ve filtreleme hızlanır|
|Sıralama yönü|`1` artan, `-1` azalan|
|`unique` özelliği|Aynı değerin tekrar eklenmesini engeller|
|`range`, `sort` destekler|Evet|
