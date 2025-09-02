
Bir veriyi **tarih (`Date`)** türüne dönüştürmek için kullanılır.

---

### 📌 Temel Söz Dizimi

```js
{ $toDate: <expression> }
```

- `<expression>`: Tarihe dönüştürmek istediğin string, timestamp, int vb.

---

## ✅ Desteklenen Dönüşüm Türleri

|Girdi Türü|Açıklama|Örnek|
|---|---|---|
|ISODate|Zaten tarih, aynen döner|`ISODate("2025-07-15T12:00:00Z")`|
|String|ISO formatındaysa tarih nesnesine dönüşür|`"2025-07-15"` → `Date`|
|Timestamp|Epoch (Unix time) değeri → `Date`|`1626307200000` → `Date`|
|Integer|Epoch time (milisaniye olarak) desteklenir|`1690000000000` → `Date`|
|Null/Boş|❌ Hata verir|`null` → ❌|
|Geçersiz str|❌ Hata verir|`"not a date"` → ❌|

---

## 🧪 Temel Örnekler

### 🎯 1. String → Date

```js
db.example.aggregate([
  {
    $project: {
      raw: "$dateStr",
      asDate: { $toDate: "$dateStr" }
    }
  }
])
```

> 📦 `"2025-07-15"` → `ISODate("2025-07-15T00:00:00Z")`

---

### 🎯 2. Epoch (milisaniye int) → Date

```js
db.timestamps.aggregate([
  {
    $project: {
      epoch: "$createdAt",
      realDate: { $toDate: "$createdAt" }
    }
  }
])
```

> 📦 `1650000000000` → `ISODate(...)`

---

### 🎯 3. Geçersiz Değer → Hata Verir

```js
db.example.aggregate([
  {
    $project: {
      asDate: { $toDate: "$notADate" }
    }
  }
])
```

> 📦 `"abc"` → ❌ HATA

---

## 🛡️ Güvenli Kullanım — `$convert` ile

```js
{
  $convert: {
    input: "$dateStr",
    to: "date",
    onError: ISODate("1970-01-01T00:00:00Z"),
    onNull: ISODate("2000-01-01T00:00:00Z")
  }
}
```

---

## 📅 Gerçek Hayat Senaryosu

### Kullanıcıların kayıt tarihini `string` formatından tarih nesnesine çevirip sıralamak:

```js
db.users.aggregate([
  {
    $project: {
      name: 1,
      registerStr: "$registeredAt",
      registerDate: { $toDate: "$registeredAt" }
    }
  },
  {
    $sort: { registerDate: -1 }
  }
])
```

> 📦 `"2024-01-20"` → `ISODate(...)` → sıralanabilir!

---

## 🧠 Özet Tablo

|Özellik|Açıklama|
|---|---|
|Ne işe yarar?|Veriyi tarih nesnesine çevirir|
|Geçerli string|`"2025-07-15"` gibi ISO formatlı|
|Epoch desteği|Evet (milisaniye olarak)|
|Hatalı string/null|HATA verir|
|Güvenli kullanım|`$convert` ile yapılır|
