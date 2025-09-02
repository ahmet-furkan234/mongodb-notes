
Verilen değeri **128-bit yüksek hassasiyetli ondalık sayı (Decimal128)** türüne dönüştürür.

> MongoDB’deki `Decimal128` türü, özellikle finansal hesaplamalar için idealdir:
> 
> - Çok büyük/small sayılar desteklenir
> - Ondalık basamak kaybı yaşanmaz

---

## 📌 Temel Söz Dizimi

```js
{ $toDecimal: <expression> }
```

- `<expression>`: String, int, double, bool vb. dönüşecek veri.

---

## ✅ Desteklenen Girdiler

|Girdi Türü|Desteklenir mi?|Açıklama|
|---|---|---|
|String|✅|`"12.345"` → `Decimal128("12.345")`|
|Int|✅|`42` → `Decimal128("42.0")`|
|Double|✅|`9.99` → `Decimal128("9.99")`|
|Boolean|✅|`true` → `1.0`, `false` → `0.0`|
|Null|❌|Hata verir|
|Geçersiz str|❌|`"abc"` gibi → Hata verir|

---

## 🧪 Temel Örnekler

### 🎯 1. String → Decimal

```js
db.example.aggregate([
  {
    $project: {
      priceStr: "$price",
      priceDecimal: { $toDecimal: "$price" }
    }
  }
])
```

> `"12.75"` → `Decimal128("12.75")`

---

### 🎯 2. Int → Decimal

```js
db.example.aggregate([
  {
    $project: {
      stock: "$quantity",
      quantityDecimal: { $toDecimal: "$quantity" }
    }
  }
])
```

> `150` → `Decimal128("150.0")`

---

### 🎯 3. Double → Decimal

```js
db.example.aggregate([
  {
    $project: {
      raw: "$score",
      scoreDecimal: { $toDecimal: "$score" }
    }
  }
])
```

> `4.95` → `Decimal128("4.95")`

---

## ⚠️ Hatalı Kullanım

```js
db.example.aggregate([
  {
    $project: {
      error: { $toDecimal: "$invalidField" }
    }
  }
])
```

- `"abc"` → ❌ Hata
- `null` → ❌ Hata

---

## 🛡️ Güvenli Kullanım – `$convert` ile

```js
{
  $convert: {
    input: "$amount",
    to: "decimal",
    onError: NumberDecimal("0.00"),
    onNull: NumberDecimal("0.00")
  }
}
```

> Böylece dönüşüm hatalarında pipeline durmaz.

---

## 💡 Nerede Kullanılır?

|Durum|Açıklama|
|---|---|
|Finansal hesaplamalar|Kuruş hassasiyetiyle işlem yapılması gereken yerler|
|Büyük parasal değerler|Büyük ondalıklı işlemlerde hata olmadan işlem|
|Parasal sıralama, toplama|`$sum`, `$avg`, `$multiply`, `$divide` içinde kullanılabilir|

---

## 🔁 Gerçek Hayat Senaryosu

### 🎯 Ürünün fiyatı string olarak tutuluyor, %18 vergiyle decimal hesaplama:

```js
db.products.aggregate([
  {
    $project: {
      fiyat: { $toDecimal: "$priceStr" },
      fiyatKDVli: {
        $multiply: [
          { $toDecimal: "$priceStr" },
          NumberDecimal("1.18")
        ]
      }
    }
  }
])
```

---

## 📐 `$toDouble` vs `$toDecimal` Karşılaştırması

|Özellik|`$toDouble`|`$toDecimal`|
|---|---|---|
|Tür|64-bit float|128-bit yüksek hassasiyetli ondalık|
|Kayıp yaşanır mı?|Evet (büyük sayılarda)|Hayır|
|Nerede kullanılır?|Genel amaçlı|Finans, muhasebe, para|
|Performans|Daha hızlı|Biraz daha yavaş ama güvenli|

---

## 🧠 Özet

|Özellik|Açıklama|
|---|---|
|Amaç|Veriyi Decimal128 ondalık sayıya çevirir|
|Desteklediği türler|String, int, double, bool|
|Null/hatalı değer|Hata verir|
|Güvenli kullanım|`$convert` ile yapılmalı|
|Nerede kullanılır?|Para, vergi, kur gibi hassas işlemler|
