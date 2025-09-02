
Herhangi bir değeri **ondalıklı (double)** sayıya dönüştürür.

> MongoDB'de `double`, JavaScript'teki `Number` veri tipine denktir (IEEE 754 float64).

---

### 📌 Temel Söz Dizimi

```js
{ $toDouble: <expression> }
```

- `<expression>`: String, int, bool, vb. veri tipi.

---

## ✅ Desteklenen Girdi Türleri

|Girdi Türü|Geçerli mi?|Açıklama|
|---|---|---|
|String|✅|`"12.34"` → `12.34`|
|Integer|✅|`7` → `7.0`|
|Boolean|✅|`true` → `1.0`, `false` → `0.0`|
|Float|✅|Zaten double olarak döner|
|Null|❌|`null` → hata|
|Hatalı String|❌|`"abc"` → hata verir|

---

## 🧪 Temel Örnekler

### 🎯 1. String → Double

```js
db.example.aggregate([
  {
    $project: {
      priceStr: "$priceStr",
      priceDouble: { $toDouble: "$priceStr" }
    }
  }
])
```

> 📦 `"19.99"` → `19.99`

---

### 🎯 2. Int → Double

```js
db.example.aggregate([
  {
    $project: {
      stockInt: "$stock",
      stockDouble: { $toDouble: "$stock" }
    }
  }
])
```

> 📦 `12` → `12.0`

---

### 🎯 3. Boolean → Double

```js
db.flags.aggregate([
  {
    $project: {
      flag: 1,
      flagDouble: { $toDouble: "$flag" }
    }
  }
])
```

> 📦 `true` → `1.0`, `false` → `0.0`

---

### ⚠️ 4. Hatalı String → Hata Verir

```js
db.test.aggregate([
  {
    $project: {
      value: "$invalidStr",
      result: { $toDouble: "$invalidStr" }
    }
  }
])
```

> Eğer `"abc"` gibi sayı olmayan bir string varsa, **aggregation hata verir**.

---

## 🛡️ Güvenli Kullanım: `$convert` ile

```js
{
  $convert: {
    input: "$priceStr",
    to: "double",
    onError: -1.0,
    onNull: 0.0
  }
}
```

> 📦 `"abc"` → `-1.0`, `null` → `0.0`

---

## 🧮 Uygulamalı Senaryo

### 🎯 Ürünün fiyatını yüzde 18 vergiyle hesapla

```js
db.products.aggregate([
  {
    $project: {
      fiyat: { $toDouble: "$priceStr" },
      fiyatKDVli: {
        $multiply: [
          { $toDouble: "$priceStr" },
          1.18
        ]
      }
    }
  }
])
```

---

## 🧠 Özet Tablo

|Özellik|Açıklama|
|---|---|
|Amaç|Değeri float (double) sayıya dönüştürmek|
|String destekler mi?|Evet, ama sadece sayı içeren string'ler|
|Ondalık korur mu?|Evet|
|Hatalı girdi ne yapar?|Hata verir (`"abc"`, `null`)|
|Alternatif|`$convert` ile güvenli dönüşüm|
