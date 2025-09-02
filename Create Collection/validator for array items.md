

Bir belge içinde bulunan dizi (örneğin `adresler`, `yorumlar`, `siparisler`) içindeki **her bir nesne** (object) belirli bir yapıya uymalıdır.

---

## 🔧 Örnek Senaryo

Aşağıdaki gibi belgelerimiz olsun:

```json
{
  "ad": "TkMatE",
  "email": "tkmate@example.com",
  "adresler": [
    { "sehir": "İstanbul", "postaKodu": "34000" },
    { "sehir": "Ankara", "postaKodu": "06000" }
  ]
}
```

Biz şunu isteriz:

- `adresler` bir **dizi** olmalı ✅
- Her eleman bir **nesne** olmalı ✅
- Her nesnede `sehir` ve `postaKodu` alanları olmalı ✅

---

## ✅ Şema: Dizi İçinde Nesne Doğrulama

```js
db.createCollection("kullanicilar", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["ad", "email", "adresler"],
      properties: {
        ad: { bsonType: "string" },
        email: {
          bsonType: "string",
          pattern: "^.+@.+\\..+$"
        },
        adresler: {
          bsonType: "array",       // ✅ Bu bir dizi
          minItems: 1,
          items: {
            bsonType: "object",    // ✅ Dizideki her eleman bir nesne olmalı
            required: ["sehir", "postaKodu"],
            properties: {
              sehir: { bsonType: "string" },
              postaKodu: {
                bsonType: "string",
                pattern: "^[0-9]{5}$"
              }
            }
          }
        }
      }
    }
  }
})
```

---

## 🔍 Açıklamalar

|Anahtar|Görev|
|---|---|
|`bsonType: "array"`|`adresler` alanı bir dizi olmalı|
|`items`|Dizideki her eleman bu yapıya uymalı|
|`minItems`|Minimum kaç eleman olmalı (opsiyonel)|
|`required`|Dizi içindeki objelerde hangi alanlar zorunlu|
|`pattern`|Posta kodunun formatı (regex)|

---

## 🚫 Geçersiz Örnek (Reddedilir)

```json
{
  "ad": "Ayşe",
  "email": "ayse@example.com",
  "adresler": [
    { "sehir": "İzmir" }  // ❌ postaKodu eksik
  ]
}
```

💥 `Document failed validation` hatası alınır.

---

## ✅ Geçerli Örnek

```json
{
  "ad": "Ali",
  "email": "ali@example.com",
  "adresler": [
    { "sehir": "Bursa", "postaKodu": "16000" },
    { "sehir": "Konya", "postaKodu": "42000" }
  ]
}
```

✅ Bu belge doğrulamayı geçer ve eklenir.

---

## 🔄 Mevcut Koleksiyona Dizi Validator Eklemek

```js
db.runCommand({
  collMod: "kullanicilar",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      properties: {
        adresler: {
          bsonType: "array",
          items: {
            bsonType: "object",
            required: ["sehir", "postaKodu"],
            properties: {
              sehir: { bsonType: "string" },
              postaKodu: {
                bsonType: "string",
                pattern: "^[0-9]{5}$"
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

## 🎯 Ekstra: Dizi İçindeki Objelerde Opsiyonel Alan

Bazı alanların opsiyonel olmasını istiyorsan `required` kısmında onları belirtmezsin:

```js
required: ["sehir"] // postaKodu opsiyonel olmuş olur
```

---

## 🔚 Özet

|Yapı|Açıklama|
|---|---|
|`bsonType: "array"`|Alanın dizi olduğunu belirtir|
|`items`|Dizideki her bir objenin şemasını tanımlar|
|`required`|Dizi elemanlarında zorunlu alanlar|
|`pattern` / `minItems`|Format ve uzunluk denetimleri|
