
MongoDB şemasızdır, ama **`validator`** ile şema benzeri kurallar tanımlanabilir.

```js
db.createCollection("koleksiyonAdi", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: [...],
      properties: { ... }
    }
  }
})
```

> Bu yapı sayesinde belirli alanların tipi, zorunluluğu, minimum değeri vs. kontrol edilir.

---

## 📌 1. `$jsonSchema` Temel Yapısı

```jsonc
{
  $jsonSchema: {
    bsonType: "object",              // Belge türü
    required: ["ad", "yas"],         // Zorunlu alanlar
    properties: {
      ad: {
        bsonType: "string",
        description: "Ad zorunludur"
      },
      yas: {
        bsonType: "int",
        minimum: 18,
        maximum: 100
      }
    }
  }
}
```

---

## 🔢 `bsonType` Türleri:

|bsonType|Açıklama|
|---|---|
|`string`|Metin|
|`int`|32-bit tam sayı|
|`long`|64-bit tam sayı|
|`double`|Ondalıklı sayı|
|`bool`|Boolean (`true/false`)|
|`object`|Nesne|
|`array`|Dizi|
|`date`|Tarih|
|`objectId`|ObjectId türü|
|`null`|Boş değer|

---

## ✅ Özellikler

### `required`:

Zorunlu alanları belirtir. Eksik olursa belge reddedilir.

```js
required: ["ad", "email"]
```

---

### `minimum` / `maximum`:

Sayısal alanlar için sınır koyar.

```js
yas: {
  bsonType: "int",
  minimum: 18,
  maximum: 99
}
```

---

### `minLength` / `maxLength`:

Metin uzunlukları için.

```js
sifre: {
  bsonType: "string",
  minLength: 6,
  maxLength: 16
}
```

---

### `pattern`:

Regex ile alanın biçimi kontrol edilir.

```js
email: {
  bsonType: "string",
  pattern: "^.+@.+\\..+$"
}
```

---

### `enum`:

Sadece belirli değerleri kabul eder.

```js
cinsiyet: {
  enum: ["erkek", "kadın", "belirtmek istemiyorum"]
}
```

---

## 🔂 Gelişmiş Örnek – Tam Kurallı

```js
db.createCollection("kullanicilar", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["isim", "email", "yas", "cinsiyet"],
      properties: {
        isim: {
          bsonType: "string",
          description: "İsim zorunlu ve string olmalı"
        },
        email: {
          bsonType: "string",
          pattern: "^.+@.+\\..+$"
        },
        yas: {
          bsonType: "int",
          minimum: 18,
          maximum: 120
        },
        cinsiyet: {
          enum: ["erkek", "kadın", "diğer"]
        },
        aktif: {
          bsonType: "bool"
        },
        kayitTarihi: {
          bsonType: "date"
        }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
})
```

---

## 🛑 `validationLevel` Seçenekleri:

|Seçenek|Açıklama|
|---|---|
|`off`|Hiçbir belge doğrulanmaz|
|`moderate`|Sadece yeni belgeler ve güncellenen alanlar doğrulanır|
|`strict`|Tüm belgeler doğrulama kurallarına tabi (varsayılan)|

---

## 🚨 `validationAction` Seçenekleri:

|Seçenek|Açıklama|
|---|---|
|`warn`|Uyarı verir ama belgeyi kabul eder|
|`error`|Kurala uymayan belgeyi reddeder ✅|

---

## 🔄 Mevcut Koleksiyona Sonradan Validator Ekleme

```js
db.runCommand({
  collMod: "kullanicilar",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["email"],
      properties: {
        email: { bsonType: "string", pattern: "^.+@.+\\..+$" }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
})
```

---

## 🧪 Validator İhlali Örneği

Eğer şöyle bir belge eklemeye çalışırsan:

```js
db.kullanicilar.insertOne({
  isim: "TkMatE",
  email: "tkmategmailcom",  // ❌ yanlış format
  yas: 25,
  cinsiyet: "erkek"
})
```

💥 MongoDB şu hatayı verir:

```
Document failed validation
```

---

## 🎯 Özet

|Özellik|Açıklama|
|---|---|
|`bsonType`|Alan türü|
|`required`|Zorunlu alanlar|
|`minimum`|Sayısal alt sınır|
|`pattern`|Regex ile kontrol|
|`enum`|Sabit değer kontrolü|
|`validationLevel`|Hangi durumlarda denetim yapılacağı|
|`validationAction`|Kurala uymayan veride ne yapılacağı|
