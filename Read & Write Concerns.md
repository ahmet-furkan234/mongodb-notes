

**Write Concern**, bir yazma işleminin (insert, update, delete) başarılı kabul edilmesi için MongoDB'nin ne kadar onay alması gerektiğini belirtir.

### 🎯 Temel Amaç:

- Yazma işlemi ne zaman başarılı sayılır?
- Ne kadar kopyaya yazılması gerekir?

---

### ✅ Ortak Write Concern Seviyeleri:

|Seviye|Açıklama|
|---|---|
|`w: 0`|Cevap beklenmez. Güvenli değil ama hızlıdır.|
|`w: 1`|Birincil düğüm yazarsa başarılı sayılır. (default)|
|`w: "majority"`|Çoğunluk replikaya yazılırsa başarılı sayılır. Daha güvenlidir.|
|`w: n`|`n` kadar replikanın yazıyı alması beklenir.|

---

### ⏳ `wtimeout`:

Yazının `w` sayıda nodelara ulaşması için maksimum bekleme süresi (ms cinsinden).

### 🔒 `j: true`:

Yazının **journal** dosyasına kaydedilmesini bekler (disk üzerine yazıldıktan sonra başarılı sayılır).

---

### 🧪 Örnek Kullanım:

```js
db.collection.insertOne(
  { isim: "Ahmet", yas: 25 },
  {
    writeConcern: {
      w: "majority",
      j: true,
      wtimeout: 1000
    }
  }
)
```

---

## 🔷 **Read Concern (Okuma Garantisi)**

**Read Concern**, bir okuma işleminin hangi verileri döndüreceğini (ne kadar “güncel” ve tutarlı olacağını) belirler.

### 🎯 Temel Amaç:

- Hangi seviyede tutarlılıkta veri okumak istiyorsun?

---

### ✅ Ortak Read Concern Seviyeleri:

|Seviye|Açıklama|
|---|---|
|`local`|En hızlısı. Yalnızca birincil node'daki mevcut veriyi okur (default).|
|`available`|Replikadan biri bile dönerse olur. Tutarlılık garantisi yok.|
|`majority`|Çoğunluk tarafından onaylanan veriyi okur. Daha tutarlı.|
|`linearizable`|Gerçek zamanlı en güncel veri. Yüksek maliyetlidir.|
|`snapshot`|Belirli bir anlık görüntüden okur. Genelde transaction içinde kullanılır.|

---

### 🧪 Örnek Kullanım:

```js
db.collection.find(
  { isim: "Ahmet" }
).readConcern("majority");
```

Alternatif olarak:

```js
db.runCommand({
  find: "collection",
  filter: { isim: "Ahmet" },
  readConcern: { level: "majority" }
})
```

---

## 🔄 Read vs Write Concern Farkı

|Özellik|Read Concern|Write Concern|
|---|---|---|
|Kullanım Alanı|Okuma işlemleri|Yazma işlemleri|
|Amaç|Okunan verinin tutarlılığı|Yazının ne kadar kalıcı olacağı|
|Güvenlik|Replika uyumu|Yazının replikalara ulaşması|
|Performans|Seviye arttıkça yavaşlar|Seviye arttıkça yavaşlar|

---

## ⚠️ Dikkat Edilecekler:

- **Write Concern çok düşükse (`w: 0`)**, yazma hataları fark edilmez.
- **Read Concern düşükse (`local`)**, henüz replikelere yayılmamış veya rollback olacak veriler görülebilir.
- Yüksek güvenlik gerekiyorsa `w: "majority"` + `j: true` kombinasyonu önerilir.
- Performans hassassa düşük seviyeler tercih edilebilir.

---

## 🔁 Uygulama Bazında Ayarlama (Node.js Örneği):

```js
const client = new MongoClient(uri, {
  writeConcern: { w: "majority", j: true },
  readConcern: { level: "majority" }
});
```
