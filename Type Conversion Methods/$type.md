
Bir alanın MongoDB içindeki **veri tipini döndürür** (string olarak).

> Böylece veriyi işlemeye başlamadan önce tip kontrolü yapılabilir.

---

## 📌 Söz Dizimi:

```js
{ $type: <expression> }
```

- `<expression>`: Tipi kontrol edilecek alan veya değer.

---

## 🔠 Geri Döndürülen Türler

`$type`, sonucu string olarak verir (bazı eski sürümler numaralı kod döndürürdü, artık çoğunlukla string).

|MongoDB Türü|$type Sonucu|
|---|---|
|String|`"string"`|
|Int32|`"int"`|
|Int64|`"long"`|
|Double|`"double"`|
|Decimal128|`"decimal"`|
|Boolean|`"bool"`|
|Date|`"date"`|
|Null|`"null"`|
|ObjectId|`"objectId"`|
|Array|`"array"`|
|Document|`"object"`|
|Undefined|`"undefined"`|
|BinData|`"binData"`|

---

## 🧪 Temel Örnekler

### 🎯 1. Tipi Belirleme

```js
db.example.aggregate([
  {
    $project: {
      value: 1,
      typeOfValue: { $type: "$value" }
    }
  }
])
```

> Örnek çıktı: `"int"`, `"string"`, `"date"`...

---

### 🎯 2. String mi? Sayı mı? Kontrol Et

```js
db.records.aggregate([
  {
    $project: {
      raw: "$score",
      isString: {
        $eq: [{ $type: "$score" }, "string"]
      }
    }
  }
])
```

> Sonuç: `true` veya `false`

---

## 🧠 Neden Kullanılır?

|Kullanım Amacı|Açıklama|
|---|---|
|Hatalı veriden kaçınmak|Tip kontrolü ile `toInt`, `toDate` hataları önlenir|
|Koşullu işlemlerde|Farklı türler için farklı işlemler yapılabilir|
|Debug amaçlı|Veri yapısını analiz etmek için kullanılır|

---

## 🔄 Uygulamalı Senaryo

### 🎯 Dönüştürülecek alan **string mi int mi** önce kontrol edip sonra işlem yap:

```js
db.payments.aggregate([
  {
    $project: {
      amount: 1,
      isString: { $eq: [{ $type: "$amount" }, "string"] },
      amountAsNumber: {
        $cond: {
          if: { $eq: [{ $type: "$amount" }, "string"] },
          then: { $toDouble: "$amount" },
          else: "$amount"
        }
      }
    }
  }
])
```

---

## 🛠️ Not — Eski Sürümde `$type` Sayı Döndürebilir

|Kod|Tür|
|---|---|
|`1`|double|
|`2`|string|
|`3`|object|
|`4`|array|
|`8`|bool|
|`9`|date|
|`10`|null|
|`16`|int|
|`18`|long|
|`19`|decimal|

Yeni sürümlerde `string` çıktıyı tercih et.

---

## 🧠 Özet

|Özellik|Açıklama|
|---|---|
|Ne işe yarar?|Veri tipini string olarak döndürür|
|Dönüşümden önce?|Evet, kullanarak güvenli hale getirebilirsin|
|Ne döner?|`"string"`, `"int"`, `"date"`, `"null"` vs.|
|Nerede kullanılır?|`$cond`, `$switch`, hata önleyici dönüşüm|
