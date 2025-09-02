
Verilen değeri **64-bit integer (long)** türüne dönüştürür.

---

### 📌 Temel Söz Dizimi

```js
{ $toLong: <expression> }
```

- `<expression>`: String, int, double, bool gibi bir değer olabilir.

---

## ✅ Desteklenen Girdi Türleri

|Girdi Türü|Çıktı (`Long`)|Açıklama|
|---|---|---|
|`String`|`Long`|`"123456789012"` → `123456789012`|
|`Int32`|`Long`|`12345` → `12345`|
|`Double`|`Long`|`12345.67` → `12345` (ondalık kesilir)|
|`Boolean`|`Long`|`true` → `1`, `false` → `0`|
|`Null` / `Invalid`|❌ Hata|`null`, `"abc"` gibi geçersiz değerlerde hata|

---

## 🧪 Temel Örnekler

### 🎯 1. String → Long

```js
db.example.aggregate([
  {
    $project: {
      strValue: "$bigNumber",
      asLong: { $toLong: "$bigNumber" }
    }
  }
])
```

> 📦 `"9223372036854775806"` → `9223372036854775806`

---

### 🎯 2. Double → Long

```js
db.example.aggregate([
  {
    $project: {
      dbl: "$price",
      priceLong: { $toLong: "$price" }
    }
  }
])
```

> 📦 `1234.56` → `1234` (kesilir)

---

### 🎯 3. Boolean → Long

```js
db.flags.aggregate([
  {
    $project: {
      flag: "$isEnabled",
      flagLong: { $toLong: "$isEnabled" }
    }
  }
])
```

> 📦 `true` → `1`, `false` → `0`

---

## ⚠️ Hatalı Kullanım — Null ve Geçersiz String

```js
db.example.aggregate([
  {
    $project: {
      bad: "$notANumber",
      result: { $toLong: "$notANumber" }
    }
  }
])
```

> `"abc"` → ❌ Hata  
> `null` → ❌ Hata

---

## 🛡️ Güvenli Kullanım — `$convert` ile

```js
{
  $convert: {
    input: "$value",
    to: "long",
    onError: NumberLong(0),
    onNull: NumberLong(-1)
  }
}
```

---

## 🔁 Gerçek Hayat Senaryosu

### Kullanıcının TC kimlik numarası `string` olarak geliyor:

```js
db.citizens.aggregate([
  {
    $project: {
      tcStr: "$TCNO",
      tcLong: { $toLong: "$TCNO" }
    }
  }
])
```

> `"12345678901"` → `12345678901` (long olarak)

---

## 📏 `$toInt` vs `$toLong` Farkı

|Özellik|`$toInt` (int32)|`$toLong` (int64)|
|---|---|---|
|Maks. Değer|2,147,483,647|9,223,372,036,854,775,807|
|Min. Değer|-2,147,483,648|-9,223,372,036,854,775,808|
|Nerede kullanılır?|Küçük sayılar|Büyük sayılar (örneğin TC, telefon, para)|

---

## 🧠 Özet Tablo

|Özellik|Açıklama|
|---|---|
|Ne işe yarar?|Veriyi 64-bit tam sayıya çevirir|
|String destekler mi?|Evet (sayısal içerikliyse)|
|Ondalık destekler mi?|Evet ama **keser**, yuvarlamaz|
|Null / hatalı veri?|❌ Hata verir|
|Güvenli yöntem|`$convert` ile yapılmalı|
