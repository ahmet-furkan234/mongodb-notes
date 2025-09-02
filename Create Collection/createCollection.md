
> Normalde `insertOne()` ile otomatik koleksiyon oluşturulabilir ama `createCollection()`:

- Özel ayarlar
- Doğrulayıcı kuralları (validator)
- Limitler, indeks kuralları gibi şeyler gerektiğinde kullanılır.

---

## ✅ Temel Söz Dizimi

```js
db.createCollection("koleksiyonAdi", options)
```

- `koleksiyonAdi`: Oluşturulacak koleksiyonun ismi
- `options`: (isteğe bağlı) gelişmiş ayarları içeren obje

---

## 🔹 En Basit Örnek:

```js
db.createCollection("ogrenciler")
```

> `ogrenciler` adında boş bir koleksiyon oluşturur

---

## 🧪 Örnek 1: Varsayılan Koleksiyon

```js
db.createCollection("kitaplar")
```

---

## 🧪 Örnek 2: `capped` Koleksiyon (Sabit Boyutlu)

```js
db.createCollection("loglar", {
  capped: true,
  size: 1048576 // 1 MB
})
```

> Bu koleksiyona sadece 1MB veri yazılabilir. FIFO yapısıyla eski veriler silinir. Log sistemlerinde kullanılır.

---

## 🧪 Örnek 3: Doğrulama (Schema Validator)

```js
db.createCollection("urunler", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["ad", "fiyat"],
      properties: {
        ad: { bsonType: "string" },
        fiyat: { bsonType: "number", minimum: 0 }
      }
    }
  }
})
```

✅ Artık sadece `ad` ve `fiyat` içeren belgeler kabul edilir. `fiyat` negatif olamaz.

---

## 🧪 Örnek 4: Belirli Alan Türleriyle Şema

```js
db.createCollection("kullanicilar", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["isim", "email"],
      properties: {
        isim: { bsonType: "string" },
        email: { bsonType: "string", pattern: "^.+@.+\\..+$" },
        yas: { bsonType: "int", minimum: 18 }
      }
    }
  }
})
```

---

## 🔍 Oluşturulan Koleksiyonları Görmek

```js
show collections
```

veya

```js
db.getCollectionNames()
```

---

## 🧨 Hata Durumu:

Aynı isimde koleksiyon varsa:

```js
{ ok: 0, errmsg: 'Collection already exists.' }
```

---

## 🧽 Koleksiyon Silme

```js
db.kitaplar.drop()
```

---

## 📋 createCollection() Opsiyonları Özeti:

|Opsiyon|Açıklama|
|---|---|
|`capped`|Sabit boyutlu koleksiyon (FIFO)|
|`size`|capped için maksimum bayt cinsinden boyut|
|`max`|Maksimum belge sayısı|
|`validator`|Şema doğrulama kuralları (`$jsonSchema` veya `$expr`)|
|`autoIndexId`|(eski sürümlerde) `_id` alanına otomatik indeks|

---

## 🎯 Tavsiye:

- Eğer sadece veri gireceksen `insertOne()` yeterlidir.
- Şema denetimi istiyorsan **`createCollection()` + validator** kullan.
