
Veride iç içe nesne varsa, bu alt nesnelere ait kuralları da şemada tanımlamak.

---

## 📦 Örnek Belge

```json
{
  "ad": "TkMatE",
  "email": "tkmate@example.com",
  "adres": {
    "sehir": "İstanbul",
    "postaKodu": "34000"
  },
  "profil": {
    "egitim": {
      "lise": "İstanbul Lisesi",
      "uni": "İTÜ"
    },
    "yas": 25
  }
}
```

---

## ✅ Validator ile Nested Object Şeması

```js
db.createCollection("kullanicilar", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["ad", "email", "adres", "profil"],
      properties: {
        ad: { bsonType: "string" },
        email: {
          bsonType: "string",
          pattern: "^.+@.+\\..+$"
        },

        adres: {
          bsonType: "object",
          required: ["sehir", "postaKodu"],
          properties: {
            sehir: { bsonType: "string" },
            postaKodu: {
              bsonType: "string",
              pattern: "^[0-9]{5}$"
            }
          }
        },

        profil: {
          bsonType: "object",
          required: ["egitim", "yas"],
          properties: {
            yas: {
              bsonType: "int",
              minimum: 18
            },
            egitim: {
              bsonType: "object",
              required: ["lise", "uni"],
              properties: {
                lise: { bsonType: "string" },
                uni: { bsonType: "string" }
              }
            }
          }
        }

      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
})
```

---

## 🔍 Açıklamalar

|Anahtar|Açıklama|
|---|---|
|`adres`|Dış objedir (nested object 1)|
|`adres.sehir`, `adres.postaKodu`|alt alanlar|
|`profil.egitim.uni`|İç içe ikinci seviye nesne (nested 2)|
|`required`|Hangi alanların zorunlu olduğunu tanımlar|

---

## ✅ Geçerli Örnek Belge

```json
{
  "ad": "Ali",
  "email": "ali@example.com",
  "adres": {
    "sehir": "Ankara",
    "postaKodu": "06000"
  },
  "profil": {
    "yas": 22,
    "egitim": {
      "lise": "Ankara Fen",
      "uni": "ODTÜ"
    }
  }
}
```

---

## ❌ Geçersiz Örnek

```json
{
  "ad": "Zeynep",
  "email": "zeynep@example.com",
  "adres": {
    "sehir": "İzmir"
  },
  "profil": {
    "yas": 21,
    "egitim": {
      "uni": "Ege Üniversitesi"
    }
  }
}
```

🔻 Hatalar:

- `adres.postaKodu` eksik
- `profil.egitim.lise` eksik

> `Document failed validation` hatası alınır.

---

## 🛠️ Mevcut Koleksiyona Nested Validator Eklemek

```js
db.runCommand({
  collMod: "kullanicilar",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      properties: {
        profil: {
          bsonType: "object",
          properties: {
            egitim: {
              bsonType: "object",
              required: ["uni"],
              properties: {
                uni: { bsonType: "string" }
              }
            }
          }
        }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
})
```

---

## 🔁 Nested + Array + Object Karma Örnek

```js
"gecmisAdresler": [
  { "sehir": "İzmir", "yil": 2018 },
  { "sehir": "Bursa", "yil": 2020 }
]
```

```js
gecmisAdresler: {
  bsonType: "array",
  items: {
    bsonType: "object",
    required: ["sehir", "yil"],
    properties: {
      sehir: { bsonType: "string" },
      yil: { bsonType: "int" }
    }
  }
}
```

---

## 📋 Validator İpuçları

|Yapı|Açıklama|
|---|---|
|`bsonType: "object"`|Alan bir nesne olmalı|
|`properties: {}`|İçindeki alt alanlar tanımlanır|
|`required: [...]`|Bu alanlar mutlaka olmalı|
|Nested object içinde de `properties` ve `required` kullanılabilir||

---

## 🎯 Özet

- Her `object` için kendi `bsonType`, `properties`, `required` tanımı yapılmalı
- Ne kadar derin olursa olsun, şema tanımı yapılabilir
- Bu yapı sayesinde MongoDB'de **SQL'e yakın veri doğruluğu** sağlanabilir

---

