
| Operatör     | Açıklama                                   |
| ------------ | ------------------------------------------ |
| `$toString`  | Herhangi bir değeri string'e çevirir       |
| `$toInt`     | Sayısal değeri tam sayıya çevirir          |
| `$toDouble`  | Sayıyı ondalıklı sayıya çevirir            |
| `$toDecimal` | Sayıyı `Decimal128` formatına çevirir      |
| `$toLong`    | Sayıyı `long` (64-bit integer) yapar       |
| `$toBool`    | Mantıksal (boolean) değere çevirir         |
| `$toDate`    | Tarih nesnesine çevirir                    |
| `$convert`   | Daha esnek ve genel dönüşüm operatörüdür   |
| `$type`      | Alanın veri türünü döndürür (kontrol için) |

---

## 📌 1. `$toString` – Her şeyi string’e çevir

```js
db.example.aggregate([
  {
    $project: {
      sayiStr: { $toString: "$number" }
    }
  }
])
```

> 📦 `number: 123` → `"123"`

---

## 📌 2. `$toInt` – Tamsayıya çevir

```js
db.example.aggregate([
  {
    $project: {
      sayiInt: { $toInt: "$stringNumber" }
    }
  }
])
```

> 📦 `"42"` → `42`

---

## 📌 3. `$toDouble` – Ondalık sayıya çevir

```js
db.example.aggregate([
  {
    $project: {
      sayiFloat: { $toDouble: "$value" }
    }
  }
])
```

> 📦 `"12.34"` → `12.34`

---

## 📌 4. `$toBool` – Boolean (true/false) yap

```js
db.example.aggregate([
  {
    $project: {
      aktifMi: { $toBool: "$isActive" }
    }
  }
])
```

> 📦 `"true"` → `true`, `"0"` → `false`

---

## 📌 5. `$toDate` – Tarih nesnesine çevir

```js
db.logs.aggregate([
  {
    $project: {
      tarih: { $toDate: "$createdAt" }
    }
  }
])
```

> 📦 `"2025-07-15"` → `ISODate("2025-07-15T00:00:00Z")`

---

## 📌 6. `$convert` – Genel dönüşüm operatörü

```js
{
  $convert: {
    input: "$value",
    to: "int",
    onError: 0,
    onNull: 0
  }
}
```

|Alan|Açıklama|
|---|---|
|`input`|Dönüştürülecek veri|
|`to`|`"int"`, `"double"`, `"string"`, `"date"`, `"bool"` gibi|
|`onError`|Dönüşüm hatası olursa dönecek değer|
|`onNull`|Giriş null ise dönecek değer|

---

## 📌 7. `$type` – Tür kontrolü

```js
{
  $project: {
    veriTipi: { $type: "$anyField" }
  }
}
```

> Örnek çıktı: `"string"`, `"int"`, `"date"`, `"bool"` vs.

---

## 🎯 Gerçek Hayat Örneği – Telefonu Integer’a Çevirme

```js
db.contacts.aggregate([
  {
    $project: {
      phoneStr: "$phone",
      phoneNum: { $toLong: "$phone" }
    }
  }
])
```

> `"5321234567"` → `5321234567` (integer olarak)

---

## ❗ Dönüşüm Hataları

Bazı dönüşümler başarısız olabilir. Örneğin `"abc"` → `$toInt` gibi. Bu tür durumlarda **`$convert` + `onError`** kombinasyonu çok kullanışlıdır.

```js
{
  $convert: {
    input: "$someField",
    to: "int",
    onError: -1,
    onNull: 0
  }
}
```

---

## 🧪 Uygulamalı Soru

```js
// "price" alanı string olarak geliyor: "19.99"
// Bunu float yapıp vergi (%18) ekleyerek yeni alan üret
db.products.aggregate([
  {
    $project: {
      fiyat: { $toDouble: "$price" },
      fiyatKDVli: {
        $multiply: [
          { $toDouble: "$price" },
          1.18
        ]
      }
    }
  }
])
```

---

## 🔚 Özet Tablo

| Amaç                  | Operatör    |
| --------------------- | ----------- |
| String'e çevir        | `$toString` |
| Int'e çevir           | `$toInt`    |
| Float'a çevir         | `$toDouble` |
| Boolean'a çevir       | `$toBool`   |
| Tarih nesnesine çevir | `$toDate`   |
| Esnek dönüşüm         | `$convert`  |
| Tür kontrol           | `$type`     |
