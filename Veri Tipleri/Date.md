
👉 **Date**, MongoDB’de tarih ve saat bilgisini tutmak için kullanılan BSON veri türüdür (type code: `0x09`).  
👉 **UTC (Coordinated Universal Time)** formatında milisaniye hassasiyetinde zamanı saklar.

➡ MongoDB Date = _64 bit signed integer_ → 1 Ocak 1970'ten (Unix epoch) itibaren milisaniye sayısı

---

## 🌟 **Özellikler**

✅ Milisaniye hassasiyetinde zaman tutar  
✅ UTC formatında saklanır  
✅ Zaman dilimi yönetimi uygulama sorumluluğundadır (Mongo UTC tutar, local time değildir)  
✅ `ISODate` gösterimi ile okunabilir hale gelir

---

## 📝 **Örnek Belge**

```json
{
  "ad": "Ahmet",
  "kayitTarihi": ISODate("2024-07-05T21:00:00Z")
}
```

👉 `kayitTarihi` MongoDB Date tipidir.

---

## ⚡ **Date Alanı Ekleme**

```js
db.kullanicilar.insertOne({
  ad: "Mehmet",
  kayitTarihi: new Date()
})
```

➡ Belge eklendiği anda geçerli tarih saat atanır.

---

## 🔍 **Sorgu Örnekleri**

### Belirli tarihten sonraki kayıtları getir

```js
db.kullanicilar.find({
  kayitTarihi: { $gt: ISODate("2024-07-01T00:00:00Z") }
})
```

### Tarih aralığında sorgu

```js
db.kullanicilar.find({
  kayitTarihi: {
    $gte: ISODate("2024-07-01T00:00:00Z"),
    $lt: ISODate("2024-07-10T00:00:00Z")
  }
})
```

---

## ⚡ **Date ile İlgili İşlemler**

### Tarih üzerinde index

```js
db.kullanicilar.createIndex({ kayitTarihi: 1 })
```

➡ Zaman bazlı sorgularda performansı artırır.

---

### Aggregation’da tarih parçalama

```js
db.kullanicilar.aggregate([
  { $project: { yil: { $year: "$kayitTarihi" }, ay: { $month: "$kayitTarihi" } } }
])
```

➡ Kayıt tarihinden yıl ve ay çıkarır.

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Date|SQL Karşılığı|
|---|---|---|
|Tip|64 bit epoch milliseconds (UTC)|`DATETIME`, `TIMESTAMP`, `DATE`|
|Zaman dilimi|UTC tutulur|Sistem ayarına bağlı (genelde yerel saat)|
|Hassasiyet|Milisaniye|Genelde saniye (bazı sistemlerde mikro/mili saniye destekli)|
|Fonksiyonlar|`$year`, `$month`, `$dayOfMonth`, `$dateToString`, `$dateAdd`|`YEAR()`, `MONTH()`, `DAY()`, `DATE_ADD()`|

---

## 🌟 **Avantajları**

✅ UTC standardında zaman tutar → global uygulamalarda uyum kolay  
✅ Milisaniye hassasiyetinde  
✅ Aggregation ile güçlü tarih parçalama, hesaplama fonksiyonları  
✅ JSON uyumlu (ISODate formatında taşınabilir)

---

## 🚩 **Dezavantajları**

❌ Zaman dilimi farklarını uygulama katmanında sen yönetirsin  
❌ Yalnızca UTC tutması lokal zaman gerektiren raporlamalarda ek iş gerektirir  
❌ Date’ler büyük koleksiyonlarda indexsiz sorgularda yavaş olabilir (index şart!)

---

## 🎯 **Ne zaman kullanılır?**

✅ Kayıt, güncelleme, silinme zamanını tutmak  
✅ Zaman bazlı raporlamalar  
✅ Belirli süre kontrolü (ör. token süresi, seans süresi)