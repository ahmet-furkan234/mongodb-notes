
Veriyi belirttiğin hedef türe **güvenli** ve **kontrollü** biçimde dönüştürmek.

---

### 📌 Temel Söz Dizimi

```js
{
  $convert: {
    input: <expression>,       // Dönüştürmek istediğin alan veya değer
    to: <targetType>,          // Hedef tip: "int", "string", "double", "bool", "date", "long", "decimal"
    onError: <value>,          // Dönüşüm hatası olursa verilecek alternatif değer (opsiyonel)
    onNull: <value>            // input null ise verilecek alternatif değer (opsiyonel)
  }
}
```

---

## 🧰 Desteklenen Hedef Tipler

|Hedef Tip|Açıklama|
|---|---|
|`"int"`|32-bit integer|
|`"long"`|64-bit integer|
|`"double"`|64-bit ondalıklı sayı|
|`"decimal"`|128-bit yüksek hassasiyet|
|`"string"`|Metin|
|`"bool"`|Boolean|
|`"date"`|Tarih|

---

## ⚠️ Önemli Özellikler

- `input` geçersiz veya dönüşüme uygun değilse **pipeline hata vermez**.
- `onError` ve `onNull` sayesinde hata durumları ve `null` için alternatif değerler belirtebilirsin.
- Hata yönetimi ile pipeline'ın durmasını önler.

---

## 🧪 Temel Örnekler

### 🎯 1. String’i Integer’a Çevir (Hatalı Değerlerde -1 Döner)

```js
db.example.aggregate([
  {
    $project: {
      safeInt: {
        $convert: {
          input: "$someString",
          to: "int",
          onError: -1,
          onNull: 0
        }
      }
    }
  }
])
```

---

### 🎯 2. String’i Date’e Çevir (Null ise 1970 Tarihi)

```js
db.example.aggregate([
  {
    $project: {
      safeDate: {
        $convert: {
          input: "$dateString",
          to: "date",
          onError: ISODate("1970-01-01"),
          onNull: ISODate("1970-01-01")
        }
      }
    }
  }
])
```

---

### 🎯 3. Sayıyı String’e Çevir

```js
db.example.aggregate([
  {
    $project: {
      strVal: {
        $convert: {
          input: "$number",
          to: "string",
          onError: "NaN",
          onNull: ""
        }
      }
    }
  }
])
```

---

## 🔁 Gerçek Hayat Senaryosu

### Kullanıcının telefon numarasını string olarak al, eğer geçerli değilse boş bırak:

```js
db.contacts.aggregate([
  {
    $project: {
      phoneStr: {
        $convert: {
          input: "$phone",
          to: "string",
          onError: "",
          onNull: ""
        }
      }
    }
  }
])
```

---

## 🧠 Özet Tablo

|Parametre|Açıklama|Opsiyonel mi?|
|---|---|---|
|`input`|Dönüştürülecek alan veya ifade|Hayır|
|`to`|Hedef tür|Hayır|
|`onError`|Hata durumunda dönecek değer|Evet (default hata)|
|`onNull`|`null` değer durumunda dönecek değer|Evet (default null)|

---

### Özetle;

`$convert`, MongoDB dönüşümlerinde **hata kontrolü ve güvenlik** sağlar. Karmaşık veri dönüşümlerinde en çok tercih edilen yöntemdir.
