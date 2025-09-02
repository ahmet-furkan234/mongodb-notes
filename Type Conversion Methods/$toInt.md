
Bir değeri **tamsayıya (int32)** dönüştürür.

---

### 📌 Temel Söz Dizimi

```js
{ $toInt: <expression> }
```

- `<expression>`: Sayı, string, boolean, date gibi dönüştürülecek alan.

---

## ✅ Desteklenen Dönüşüm Türleri

|Girdi Türü|Dönüştürülebilir mi?|Örnek|
|---|---|---|
|String|✅|`"123"` → `123`|
|Double/Float|✅|`12.75` → `12` (ondalık **kırpılır**)|
|Boolean|✅|`true` → `1`, `false` → `0`|
|Null|❌ (hata verir)|`null` → ❌|
|Geçersiz String|❌ (hata verir)|`"abc"` → ❌|
|Tarih (Date)|❌|`ISODate(...)` → ❌|

---

## 🧪 Temel Örnekler

### 🎯 1. String → Int

```js
db.test.aggregate([
  {
    $project: {
      original: "$priceStr",
      price: { $toInt: "$priceStr" }
    }
  }
])
```

> 📦 `"150"` → `150`

---

### 🎯 2. Float (Double) → Int

```js
db.test.aggregate([
  {
    $project: {
      original: "$rating",
      ratingInt: { $toInt: "$rating" }
    }
  }
])
```

> 📦 `4.8` → `4`  
> 💡 Ondalık **kısmı kesilir**, yuvarlama yapılmaz.

---

### 🎯 3. Boolean → Int

```js
db.test.aggregate([
  {
    $project: {
      aktif: "$isActive",
      aktifInt: { $toInt: "$isActive" }
    }
  }
])
```

> `true` → `1`, `false` → `0`

---

## ⚠️ Hatalı Kullanım — Geçersiz Değerler

```js
db.test.aggregate([
  {
    $project: {
      sayi: { $toInt: "$someString" }
    }
  }
])
```

Eğer `someString = "abc"` veya `null` ise bu durumda **aggregation pipeline hata verir.**

---

## 🛡️ Güvenli Kullanım: `$convert` ile

Eğer hata almak istemiyorsan `$convert` operatörünü kullan:

```js
{
  $convert: {
    input: "$someString",
    to: "int",
    onError: -1,
    onNull: 0
  }
}
```

---

## 🔁 Uygulamalı Senaryo

### 🔢 Telefon numarasının son 4 hanesini int olarak almak:

```js
db.contacts.aggregate([
  {
    $project: {
      last4Digits: {
        $toInt: {
          $substrCP: [{ $toString: "$phone" }, 7, 4]
        }
      }
    }
  }
])
```

---

## 🧠 Özet Tablo

|Özellik|Açıklama|
|---|---|
|Amaç|Değeri integer'a dönüştürür|
|String destekli mi?|Evet, sayı içeren string'ler|
|Ondalık destekli mi?|Evet, ama **kırpar**|
|Hatalı girdi|Hata verir (`"abc"`, `null`)|
|Alternatif|`$convert` ile güvenli dönüşüm|
