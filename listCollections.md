
Aktif veritabanındaki **mevcut koleksiyonları (tabloları)** listelemek.

> SQL’deki `SHOW TABLES` karşılığıdır.

---

## 🔹 Temel Kullanım:

```js
db.getCollectionNames()        // eski yöntem (hala çalışır)
db.getSiblingDB("veritabani_adi").getCollectionNames()
```

🔧 Modern ve detaylı yöntem:

```js
db.getCollectionInfos()
db.getCollectionInfos({ name: "kitaplar" })   // filtreli
```

🔍 Daha kapsamlı:

```js
db.listCollections()
```

> Bu komut **cursor döndürür**. Listelemek için `.toArray()` kullanılır:

```js
db.listCollections().toArray()
```

---

## 🧪 Örnek:

Varsayalım ki aktif veritabanın `kütüphane` ve içinde birkaç koleksiyon var.

```js
use kütüphane

db.listCollections().toArray()
```

🔽 Örnek çıktı:

```json
[
  {
    name: "kitaplar",
    type: "collection",
    options: {},
    info: { readOnly: false },
    idIndex: { key: { _id: 1 }, name: "_id_" }
  },
  {
    name: "kullanicilar",
    type: "collection"
  }
]
```

---

## 🔍 Filtreli Kullanım

Sadece adı `"kitaplar"` olan koleksiyonu getir:

```js
db.listCollections({ name: "kitaplar" }).toArray()
```

---

## 📌 Alternatif:

```js
db.getCollectionNames()  // ["kitaplar", "kullanicilar"]
```

> Sadece adları döndürür (dizi olarak), `.toArray()` gerekmez.

---

## 🔧 Sistem Koleksiyonları Gösterme

```js
db.listCollections({ name: { $regex: /^system\./ } }).toArray()
```

> `system.indexes` gibi MongoDB'nin dahili koleksiyonlarını da listeler.

---

## 🧠 Karşılaştırmalı Tablo:

|Komut|Ne Döner?|Açıklama|
|---|---|---|
|`db.listCollections()`|Cursor|Koleksiyon meta bilgileri|
|`.toArray()`|Dizi|Listeyi görmek için şarttır|
|`db.getCollectionNames()`|Dizi (sadece adlar)|Sadece koleksiyon isimleri|
|`db.getCollectionInfos()`|Meta veri dizisi|Modern sürüm, filtreleme yapılabilir|

---

## 🧹 Not:

Eğer henüz koleksiyon oluşturulmadıysa, `listCollections()` boş döner.

---

## 🎯 Bonus: Koleksiyonları sırayla yazdır

```js
db.listCollections().toArray().forEach(function(coll) {
    print(coll.name);
});
```

---

## 🚀 Özet:

|İhtiyaç|Kullanılacak Komut|
|---|---|
|Tüm koleksiyonları listele|`db.listCollections().toArray()`|
|Sadece adları listele|`db.getCollectionNames()`|
|Belirli koleksiyonu filtrele|`db.listCollections({ name: "x" })`|

---

İstersen şimdi koleksiyon silme (`drop()`), koleksiyon oluşturma (`createCollection()`), ya da **koleksiyon şeması/validator** konusuna geçebiliriz.

Hangisini seçelim TkMatE?