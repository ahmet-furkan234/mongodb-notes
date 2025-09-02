
## 🧠 Kural:

> Veritabanı ancak **ilk koleksiyon** veya belge eklenince fiziksel olarak oluşturulur.

---

## ✅ Kullanım Aşamaları:

### 🔹 1. `use <veritabani_adi>`

```js
use myNewDb
```

Bu komut:

- Eğer `myNewDb` adında bir veritabanı varsa → ona geçer.
- Yoksa → geçici olarak oluşturur ama fiziksel olarak henüz **yoktur**.

---

### 🔹 2. İlk koleksiyon veya veri eklendiğinde oluşur:

```js
db.users.insertOne({ name: "TkMatE", age: 25 })
```

> Artık `myNewDb` veritabanı **gerçekten** oluşturulmuştur.

---

## 🧪 Örnek:

```js
use ogrenciSistemi

db.ogrenciler.insertOne({
  ad: "Ahmet",
  soyad: "Yılmaz",
  yas: 20,
  bolum: "Bilgisayar Mühendisliği"
})
```

✅ Şimdi `ogrenciSistemi` adında bir veritabanı var, içinde `ogrenciler` koleksiyonu ve 1 belge var.

---

## 🔍 Var olan veritabanlarını listele:

```js
show dbs
```

> Sadece içine veri yazılmış veritabanları burada görünür.

---

## 🔍 Aktif veritabanı:

```js
db
```

> Şu an hangi veritabanında olduğunu gösterir.

---

## ❌ Veritabanı silme:

```js
use ogrenciSistemi
db.dropDatabase()
```

> Aktif veritabanını tamamen siler. Dikkat!

---

## 📌 Notlar:

|Komut|Açıklama|
|---|---|
|`use <db>`|DB seçer veya geçici oluşturur|
|`db.<collection>.insertOne()`|Veri ekler ve fiziksel oluşumu tetikler|
|`show dbs`|Oluşturulmuş veritabanlarını gösterir|
|`db`|Aktif veritabanını gösterir|
|`db.dropDatabase()`|Aktif veritabanını siler|

---
## 🎓 Alıştırma

```js
use kütüphane

db.kitaplar.insertOne({
  ad: "1984",
  yazar: "George Orwell",
  tür: "Distopya",
  sayfa: 328
})
```

> Şimdi MongoDB'de `kütüphane` veritabanı ve `kitaplar` koleksiyonu oluştu.

