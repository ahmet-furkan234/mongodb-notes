
MongoDB'de veritabanı performansını optimize etmek için aşağıdaki alanlarda iyileştirmeler yapılabilir:

---

## 🧩 1. **Indexing (İndeksleme)**

### 🔹 Amaç:

- Sorguların daha hızlı çalışması.
- Full Collection Scan (tüm veriyi tarama) yerine doğrudan ilgili belgeye gitmek.

### 📌 Uygulamalar:

- **Single Field Index** → En sık filtrelenen alanlarda
- **Compound Index** → Birden fazla alanla yapılan sorgularda
- **TTL Index** → Geçici verilerin yönetimi
- **Text Index** → Metin aramalarında
- **Geospatial Index** → Konuma dayalı sorgular
- **Covered Query** → Hem filtreleme hem projeksiyon sadece index üzerinden yapılabiliyorsa çok hızlıdır.

---

## 🧩 2. **Query Optimization (Sorgu İyileştirme)**

### 📌 İpuçları:

- Sorgularda **gereksiz alanları getirme**:
    ```js
    db.users.find({}, { name: 1, email: 1 }) // sadece gerekli alanları getir
    ```

- **$regex, $where** gibi yavaş operatörlerden kaçın
- **Index ile uyumlu** sorgular yaz:

```js
db.users.find({ age: { $gt: 18 } }) // age alanı indexli olmalı
```

- **$in** yerine mümkünse **eşitlik filtresi** kullan

---

## 🧩 3. **Schema Design (Şema Tasarımı)**

### 📌 Uygulamalar:

- Sorgu bazlı şema tasarımı: Verinin nasıl kullanılacağını düşünerek model oluştur.
- **Embed vs Reference**:
    - Sık birlikte kullanılan belgeleri **embed** etmek (iç içe) daha hızlıdır. 
    - Nadir ilişkilerde **referans** (normalize) kullanılır.   
- Gereksiz veri tekrarlarından kaçın.

---

## 🧩 4. **Aggregation Pipeline İyileştirme**

### 📌 Stratejiler:

- `$match` ve `$project` aşamalarını pipeline’ın başına koy (daha az veri işler)
- Gereksiz `$group` kullanımlarından kaçın
- `$limit`, `$skip`, `$sort` işlemleri **index destekli** olmalı

---

## 🧩 5. **Server Side Ayarlar**

### 📌 Ayarlanabilir Parametreler:

- WiredTiger cache ayarları
- Max connections
- Query timeout
- Profiler ile ağır sorguların izlenmesi:

```js
db.setProfilingLevel(2)
```

---

## 🧩 6. **Sharding (Yatay Parçalama)**

### 📌 Ne Zaman?

- Veri çok büyükse
- Tek sunucunun kapasitesini aşıyorsa
- Yük dengeleme gerekiyorsa

> Sharding ile veriler shard key’e göre parçalara ayrılır. Yük farklı sunuculara dağılır.

---

## 🧩 7. **Replication (Yedekleme & Yüksek Erişilebilirlik)**

- Performans değil ama **kesintisizlik** sağlar.
- Okuma yükü **secondary replica**'lara dağıtılabilir (`readPreference` ayarlanarak).

---

## 🧩 8. **Monitoring & Tools**

### Araçlar:

|Araç|Açıklama|
|---|---|
|**Atlas Performance Advisor**|Yavaş sorguları önerilerle sunar|
|**`explain()`**|Sorgunun çalışma planını gösterir|
|**MongoDB Profiler**|Ağır sorguları analiz eder|
|**Compass**|UI üzerinden analiz imkanı sağlar|

---

## 🔬 `explain()` Örneği

```js
db.orders.find({ status: "shipped" }).explain("executionStats")
```

> Bu sorgunun nasıl çalıştığını, kaç belge taradığını, index kullanıp kullanmadığını gösterir.

---

## 🔧 İpuçları Tablosu

|Optimizasyon Alanı|Öneri|
|---|---|
|Index|Sık kullanılan alanlara index ekle|
|Sorgular|Sadece gerekli veriyi getir, index uyumlu yaz|
|Şema|Embed / Referans kararını doğru ver|
|Aggregation|`$match` ve `$project` en başta olmalı|
|Sharding|Büyük veri kümelerinde kullan|
|Monitoring|`explain()`, Profiler, Atlas Advisor kullan|

---

## 📈 Performans Tespiti Nereden Başlamalı?

1. **Yavaş sorguların logları veya profiler çıktıları**
2. `db.collection.stats()` → koleksiyon büyüklüğü, index sayısı
3. `db.collection.explain()` → sorgu detayları
4. **Atlas kullanıyorsan Performance Advisor önerileri**
