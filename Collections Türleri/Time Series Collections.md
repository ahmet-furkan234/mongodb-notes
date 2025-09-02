
- Zaman serisi koleksiyonları, **zamanla değişen ve sürekli veri üreten uygulamalar için optimize edilmiş koleksiyon türüdür.**
- Özellikle IoT verileri, sensör ölçümleri, loglar, finansal zaman serileri gibi durumlarda kullanılır.
- MongoDB 5.0 ve sonrasında native destek sunar.

---

## ⚙️ Özellikleri

|Özellik|Açıklama|
|---|---|
|Zaman alanı|Zaman bilgisini tutan alan (örn: timestamp)|
|Meta alanı|Zaman serisi veriye ait açıklayıcı bilgiler|
|Otomatik sıkıştırma|Veriler zamanla sıkıştırılır, disk kullanımı azalır|
|Partitioning|Veriler otomatik olarak zaman dilimlerine ayrılır|
|Performans|Zaman serisi veri sorgularında optimize|
|Index yönetimi|Otomatik olarak zaman alanına index oluşturur|

---

## 🛠️ Oluşturma

Örnek: Sensör verileri için zaman serisi koleksiyonu oluşturalım.

```js
db.createCollection("sensorData", {
  timeseries: {
    timeField: "timestamp",   // Zaman bilgisini tutan alan
    metaField: "metadata",    // İsteğe bağlı meta alan
    granularity: "seconds"    // Veri granülerliği (seconds, minutes, hours)
  }
})
```

- `timeField`: Zaman bilgisinin tutulduğu alan adı zorunludur.
- `metaField`: (Opsiyonel) Ölçümün ait olduğu cihaz, lokasyon gibi bilgiler.
- `granularity`: Verinin zaman çözünürlüğü, performansı etkiler.

---

## 📋 Örnek Veri Ekleme

```js
db.sensorData.insertMany([
  {
    timestamp: new Date("2025-07-12T10:00:00Z"),
    metadata: { deviceId: "sensor01", location: "room1" },
    temperature: 22.5,
    humidity: 60
  },
  {
    timestamp: new Date("2025-07-12T10:01:00Z"),
    metadata: { deviceId: "sensor01", location: "room1" },
    temperature: 22.7,
    humidity: 58
  }
])
```

---

## 🔍 Sorgulama

```js
db.sensorData.find({
  "metadata.deviceId": "sensor01",
  timestamp: { $gte: new Date("2025-07-12T10:00:00Z") }
})
```

---

## 📈 Avantajları

- Zaman serisi veriye özel optimize edilmiş depolama ve indeksleme.
- Otomatik veri sıkıştırma ile disk alanı tasarrufu.
- Büyük veri kümelerinde yüksek sorgu performansı.
- Zaman bazlı partition yapısı sayesinde veri yönetimi kolaylığı.

---

## 📌 Dikkat Edilmesi Gerekenler

- `timeField` mutlaka belirtilmelidir.
- `metaField` opsiyoneldir ancak sorgularda kullanışlıdır.
- Eski MongoDB sürümlerinde native time series koleksiyon yoktur.
- Aggregation pipeline ile zaman serisi analizleri yapılabilir.

---

## Özet Tablosu

|Özellik|Açıklama|
|---|---|
|Amaç|Zaman serisi veriler için optimize edilmiş koleksiyon|
|`timeField`|Zaman bilgisinin tutulduğu alan|
|`metaField`|Meta veri alanı (opsiyonel)|
|Performans|Zaman bazlı veri sorgularında yüksek|
|Desteklenen Sürüm|MongoDB 5.0 ve sonrası|
