
`$currentDate`, bir dökümanı güncellerken alanlara **anlık tarih/saat (`new Date()`) veya timestamp (long)** atamak için kullanılır.

---

## 📌 Kullanım Formatı

```js
{ $currentDate: { <alanAdı>: <true veya { $type: "timestamp" }> } }
```

---

## ✅ 1. Temel Kullanım (Tarih ekle)

```js
db.kullanicilar.updateOne(
  { ad: "Ali" },
  { $currentDate: { sonGiris: true } }
)
```

📌 Bu işlem şunu yapar:

- `sonGiris` alanını ekler veya günceller
- Değer olarak `new Date()` (yani `ISODate`) atanır

---

## 🧪 2. Timestamp Kullanımı

```js
db.kullanicilar.updateOne(
  { ad: "Ali" },
  { $currentDate: { sonGirisTS: { $type: "timestamp" } } }
)
```

📌 Bu işlem:

- `sonGirisTS` alanına BSON `Timestamp` türünde veri atar
- Sadece MongoDB iç süreçlerinde kullanılır (ör: replika setleri için)    

---

## 🧾 3. `$set` ile Birlikte Kullanım

```js
db.loglar.updateOne(
  { _id: ObjectId("...") },
  {
    $set: { mesaj: "Güncellendi" },
    $currentDate: { guncellenmeTarihi: true }
  }
)
```

---

## 🧮 4. Çoklu Alan Güncelleme

```js
db.loglar.updateOne(
  { _id: ObjectId("...") },
  {
    $currentDate: {
      guncellenmeTarihi: true,
      sistemZamani: { $type: "timestamp" }
    }
  }
)
```

---

## 📊 `$currentDate` Kullanım Alanları

|Alan|Açıklama|
|---|---|
|`createdAt`|İlk kayıt sırasında `new Date()` verilebilir|
|`updatedAt`|Her güncellemede `$currentDate` ile güncellenebilir|
|`lastLogin`, `lastModified`|Otomatik zaman güncellemeleri|
|`timestamp`|Versiyon, replika zaman bilgisi, log kontrolü|

---

## ❗ Dikkat Edilecek Noktalar

- `$currentDate` **sadece `update` içinde kullanılır** (`insert` için geçerli değildir)
- Değer `true` olduğunda varsayılan olarak `Date` atanır
- Eğer `$type: "timestamp"` kullanırsan BSON Timestamp atanır (bu genellikle sistem içi kullanımlar içindir)

---

## 🔁 Otomatik `updatedAt` için pratik örnek (Mongoose tarafında da yaygın)

```js
db.urunler.updateOne(
  { stokKodu: "ABC123" },
  {
    $set: { fiyat: 49.90 },
    $currentDate: { updatedAt: true }
  }
)
```

---

## 🧠 `$currentDate` vs `new Date()`

|Yöntem|Kullanım Yeri|Özellik|
|---|---|---|
|`new Date()`|`insert`, `update`, JS tarafı|Değer manuel atanır|
|`$currentDate`|Sadece `update`|Güncel tarih/timestamp otomatik atanır|

---

## 📌 Özet

|Komut|Anlamı|
|---|---|
|`{ $currentDate: { tarih: true } }`|`tarih` alanına şu anki `Date` nesnesini ata|
|`{ $currentDate: { zaman: { $type: "timestamp" } } }`|`zaman` alanına BSON Timestamp ata|
|`$set` ile birlikte|İçeriği ve tarihi aynı anda güncelle|
