
- **Sabit boyutlu**, yani disk üzerinde önceden belirlenmiş bir **maksimum boyuta** sahip koleksiyondur.
- Boyutu dolduğunda, **en eski belgeler otomatik olarak silinir** ve yeni belgeler eklenir.
- FIFO (First In First Out) mantığıyla çalışır.
- Performans açısından hızlıdır, çünkü koleksiyon dosyası sabit büyüklüktedir.
- Genellikle **loglama, olay takibi, kuyruk sistemleri** gibi durumlarda tercih edilir.

---

## ⚙️ Özellikleri

|Özellik|Açıklama|
|---|---|
|Boyut|Oluşturulurken belirlenir (byte cinsinden)|
|Veri Silme|Boyut aşıldığında en eski veriler otomatik silinir|
|Düzen|Belgeler yazıldığı sıraya göre saklanır (insertion order)|
|Güncelleme|Belge boyutu değiştirilemez, çünkü yer sabittir|
|Performans|Sabit boyut sayesinde yüksek yazma performansı sağlar|

---

## 🛠️ Oluşturma

```js
db.createCollection("logs", { capped: true, size: 5242880 })  // 5 MB
```

- `capped: true` özelliği sabit boyutlu koleksiyon yapar.
- `size` bayt cinsindendir, koleksiyonun maksimum boyutunu belirtir.

---

## 🧩 Opsiyonel Parametre: `max`

- Koleksiyona maksimum kaç belge olacağını sınırlandırmak için kullanılır.

```js
db.createCollection("logs", { capped: true, size: 5242880, max: 5000 })
```

- Koleksiyon boyutu dolmasa bile, belge sayısı `max`’a ulaştığında en eski belgeler silinir.

---

## 🔍 Örnek Kullanım Senaryosu

```js
db.logs.insertMany([
  { level: "info", message: "System started", timestamp: new Date() },
  { level: "error", message: "Connection failed", timestamp: new Date() },
  // ...
])
```

- Loglar sürekli eklenir.
- Boyut sınırı dolunca en eski loglar otomatik temizlenir.

---

## 📌 Dikkat Edilmesi Gerekenler

- Capped koleksiyonlarda **belgeler üzerinde güncelleme yapılırken** belge boyutu değiştirilemez.
- Eğer büyürse, hata alırsın.
- Capped koleksiyonlar **sadece insert ve update (boyut değişmeyen) işlemlerine uygundur**.
- `drop` ve `rename` gibi işlemler desteklenir.

---

## ⚡ Performans Avantajları

- Disk üzerinde sürekli büyüme olmadığı için **fragmentasyon olmaz**.
- **Yazma işlemleri hızlıdır**.
- Genellikle **kuyruk, log ve event stream** uygulamalarında kullanılır.

---

## 📊 Özet Tablosu

|Özellik|Capped Collection|
|---|---|
|Boyut|Sabit (örnek: 5 MB)|
|Eski Veri|Boyut dolunca otomatik silinir|
|Güncelleme|Boyut değişikliği yapılamaz|
|Performans|Yüksek (disk fragmentasyonu yoktur)|
|Kullanım Alanı|Log, kuyruk, zaman serisi verileri|

---

İstersen şimdi **TTL index** ile devam edebiliriz ya da capped collection üzerinde pratik CRUD örnekleri yapabiliriz. Hangi konuda devam etmek istersin?