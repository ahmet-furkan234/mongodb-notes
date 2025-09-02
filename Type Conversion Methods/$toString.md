
Verilen herhangi bir değeri **string (metin)** türüne dönüştürür.

---

## 📌 Temel Söz Dizimi:

```js
{ $toString: <expression> }
```

- `<expression>`: Dönüştürmek istediğin sayı, tarih, bool, vb.
    

---

## 💡 Desteklenen Dönüşümler

|Dönüştürülebilir Tür|Açıklama|Örnek|
|---|---|---|
|`int` / `double`|Sayılar → string|`42` → `"42"`|
|`bool`|`true`/`false` → string|`true` → `"true"`|
|`date`|Tarihler → string|`ISODate(...)` → `"2025-07-15T00:00:00Z"`|
|`ObjectId`|ObjectId → string|`ObjectId("...")` → `"60a..."`|

---

## 🧪 Örnekler

### 🎯 1. Sayıyı String'e Çevir

```js
db.numbers.aggregate([
  {
    $project: {
      original: "$number",
      stringVersion: { $toString: "$number" }
    }
  }
])
```

> 📦 `number: 123` → `"123"`

---

### 🎯 2. Tarihi String'e Çevir

```js
db.logs.aggregate([
  {
    $project: {
      rawDate: "$createdAt",
      dateAsString: { $toString: "$createdAt" }
    }
  }
])
```

> 📦 `ISODate("2025-07-15")` → `"2025-07-15T00:00:00Z"`

---

### 🎯 3. Boolean Değerleri String'e Çevir

```js
db.flags.aggregate([
  {
    $project: {
      flag: 1,
      flagStr: { $toString: "$flag" }
    }
  }
])
```

> 📦 `true` → `"true"`, `false` → `"false"`

---

### 🎯 4. ObjectId'yi String'e Çevir

```js
db.users.aggregate([
  {
    $project: {
      _id: 1,
      idAsString: { $toString: "$_id" }
    }
  }
])
```

> 📦 `ObjectId("662b...")` → `"662b..."`

---

## ❗ Dikkat

- `$toString`, `null` veya boş değer geldiğinde **null döndürür**, hata vermez.
- Eğer bir nested field varsa (`address.zipCode` gibi), önce alanın var olduğundan emin ol (`$ifNull` ile).

---

## 🛡️ Güvenli Kullanım (Hata Önleyici)

```js
{
  $toString: { $ifNull: ["$someField", ""] }
}
```

---

## 🎯 Gerçek Hayat Senaryosu

### Telefon numarasını metin olarak formatlamak:

```js
db.contacts.aggregate([
  {
    $project: {
      phone: "$phone",
      phoneStr: {
        $concat: [
          "+90 ",
          { $substrCP: [{ $toString: "$phone" }, 0, 3] },
          " ",
          { $substrCP: [{ $toString: "$phone" }, 3, 3] },
          " ",
          { $substrCP: [{ $toString: "$phone" }, 6, 2] },
          " ",
          { $substrCP: [{ $toString: "$phone" }, 8, 2] }
        ]
      }
    }
  }
])
```

---

## 🧠 Özet

|Özellik|Açıklama|
|---|---|
|Ne yapar?|Değeri string'e çevirir|
|Kullanımı kolay mı?|Evet — tek satırda çalışır|
|Ne zaman kullanılır?|Sayı, tarih, bool gibi değerleri yazı olarak kullanmak istediğinde|
|Hatalı değer?|`null` döndürür, hata vermez|
