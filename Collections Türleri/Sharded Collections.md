
- **Sharding**, MongoDB’de veritabanını yatay ölçeklendirme (horizontal scaling) yöntemi.
- Büyük veri kümelerini ve yüksek trafik alan uygulamaları yönetmek için kullanılır.
- Veriler **shard key** (parçalama anahtarı) bazında birden fazla fiziksel sunucuya (shard) dağıtılır.
- Bu sayede sorgular ve yazma işlemleri **birden çok sunucuya paralel dağılır**, performans ve kapasite artar.

---

## ⚙️ Özellikleri

|Özellik|Açıklama|
|---|---|
|Veriler Parçalanır|Shard key’e göre farklı sunuculara dağılır|
|Yatay Ölçeklendirme|Yeni sunucu ekleyerek kapasite artırılır|
|Şeffaf Erişim|Uygulama için tek veritabanı gibi görünür|
|Performans Artışı|Sorgu ve yazma yükü dağıtılır|
|Yüksek Erişilebilirlik|Replika setleri ile birlikte kullanılır|

---

## 🛠️ Sharding Mimarisi Temelleri

- **Config Server:** Sharding metadata bilgilerini tutar.
- **Shard:** Verinin parçalarının saklandığı fiziksel sunucu veya replika seti.
- **Query Router (mongos):** İstemciden gelen sorguları shard’lara yönlendirir.

---

## 📌 Sharding Adımları (Özet)

1. **Config server kümesi kurulur.**
2. **Shard sunucuları kurulur (replika set önerilir).**
3. **MongoS query router başlatılır.**
4. **Veritabanı sharding için etkinleştirilir:**

```js
sh.enableSharding("myDatabase")
```

5. **Shard key seçilir ve koleksiyon shard edilir:**

```js
sh.shardCollection("myDatabase.myCollection", { shardKeyField: 1 })
```

---

## ⚠️ Shard Key Seçimi

- **Shard key çok önemli!**
- Veri dağılımını dengeler.
- Sorgu performansını etkiler.
- Yüksek kardinaliteli, sık sorgulanan ve yazılan alanlar tercih edilir.

---

## 🔍 Sharded Collection Kullanımı

- Uygulama tarafından normal koleksiyon gibi kullanılır.
- `mongos` router tüm shard’ları yönetir.
- Sorgular shard key kullanıyorsa, direkt ilgili shard’a gider.
- Shard key olmadan sorgular tüm shard’lara broadcast edilir, yavaşlar.

---

## 📈 Avantajlar

- Çok büyük veri kümelerinde performans ve ölçeklenebilirlik.
- Sorgu ve yazma yükünün dağıtılması.
- Yüksek erişilebilirlik (replika set ile birlikte).

---

## 📋 Özet Tablosu

|Özellik|Açıklama|
|---|---|
|Amaç|Yatay ölçeklendirme ve büyük veri yönetimi|
|Veri Dağılımı|Shard key’e göre sunuculara dağıtılır|
|Kullanıcı Görünümü|Tek veritabanı gibi davranır|
|Performans|Yüksek, doğru shard key ile optimize|
|Karmaşıklık|Kurulum ve yönetim karmaşıktır|
