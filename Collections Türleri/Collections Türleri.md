
# MongoDB Collections Türleri ve Özellikleri

MongoDB’de veriler **collections** içinde tutulur. Collections, SQL’deki tablolar gibi düşünebilirsin ama daha esnek ve şeması olmayan yapılar.

---

## 1. Normal Collection (Standart Koleksiyon)

- En yaygın koleksiyon türüdür.
- İçerisinde BSON belgeleri (dokümanlar) tutulur.
- Şema kısıtlaması yoktur (şemasız yapı).
- Veri ekleme, güncelleme, silme işlemleri yapılabilir.

```js
db.createCollection("normalCollection")
```

---

## 2. Capped Collection (Sınırlı Boyutlu Koleksiyon)

- Boyutu sabittir, belirli MB/GB kadar yer kaplar.
- Veri eklenirken eski belgeler otomatik silinir (dairesel buffer gibi).
- Yazma işlemleri hızlıdır.
- Sıralı veri ve loglar için uygundur.

### Özellikleri:

|Özellik|Açıklama|
|---|---|
|Boyut|Sabit, aşılırsa eski silinir|
|Düzen|Ekleme sırasına göre korunur|
|Güncelleme|Aynı belge boyut değiştirilemez (sabit)|

### Oluşturma:

```js
db.createCollection("myCapped", { capped: true, size: 100000 })
```

---

## 3. TTL Collection (Time To Live)

- Aslında TTL koleksiyon değil, **TTL index** kullanımıdır.
- Belirli bir süre sonra otomatik olarak belgeler silinir.
- Genellikle oturum, cache, log gibi geçici veriler için kullanılır.

### Nasıl Çalışır?

1. Bir tarih alanına (örneğin `createdAt`) TTL index oluşturulur.
2. MongoDB otomatik olarak **bu tarihten sonra belirlenen saniye kadar geçen belgeleri siler**.

### Örnek:

```js
db.sessions.createIndex({ "createdAt": 1 }, { expireAfterSeconds: 3600 })
```

- `createdAt` alanı 1 saatten eski olan belgeler otomatik silinir.

---

## 4. System Collections

- MongoDB’nin iç işlemleri için kullandığı özel koleksiyonlardır.
- Örneğin `system.indexes`, `system.profile`, `system.users`.
- Normal kullanıcı işlemlerinde değiştirilmez veya kullanılmaz.

---

## 5. Views (Görünümler)

- Gerçek veri içermez, başka koleksiyonların sorgu sonucu olan **sanallaştırılmış koleksiyonlardır**.
- SQL’deki **View** gibi düşünülebilir.
- Okuma için optimize edilmiştir, yazma yapılamaz.

### Oluşturma:

```js
db.createView("activeUsers", "users", [ { $match: { active: true } } ])
```

---

## 6. Time Series Collections (Zaman Serisi Koleksiyonları)

- Özellikle zaman serisi verisi için optimize edilmiştir.
- IoT, loglama, telemetri gibi zaman bazlı veriler için kullanılır.
- Otomatik olarak veri sıkıştırma, otomatik zaman bazlı partition (segment) yapar.

### Oluşturma:

```js
db.createCollection("sensorData", {
  timeseries: {
    timeField: "timestamp",
    metaField: "metadata",
    granularity: "seconds"
  }
})
```

---

## 7. Sharded Collections (Parçalanmış Koleksiyonlar)

- Büyük veri kümeleri için yatay ölçeklendirme (sharding) uygulanmış koleksiyonlardır.
- Veriler **shard key**’e göre farklı sunuculara dağıtılır.
- Büyük ve yüksek trafikli uygulamalarda kullanılır.

---

# Özet Tablosu

|Collection Türü|Kullanım Amacı|Özellikler|
|---|---|---|
|Normal|Genel veri saklama|Şemasız, esnek|
|Capped|Log, kuyruk, sabit boyutlu veri|Sabit boyut, döngüsel|
|TTL (index)|Geçici veri, cache|Otomatik eski veri silme|
|System Collections|Sistem içi|Kullanıcı genelde değiştirmez|
|Views|Sanal koleksiyon|Sadece okuma, sorgu tabanlı|
|Time Series|Zaman serisi veri|Zaman bazlı optimizasyonlar|
|Sharded|Büyük ölçekli dağıtık veri|Veriler parçalanır|

---

İstersen TTL index örnekleri veya capped collection kullanımı üzerine uygulama örnekleri yapabiliriz, ya da diğer koleksiyon türlerinden derinlemesine devam edebiliriz. Ne dersin?