
`$function`, **MongoDB 4.4 ve sonrasında gelen** güçlü bir operatördür. Aggregation pipeline içinde **JavaScript fonksiyonları çalıştırmanı** sağlar.

MongoDB'nin yerleşik fonksiyonlarıyla yapılamayan özel işlemler için kullanılır.

---

### 🧠 Temel Söz Dizimi

```js
{
  $function: {
    body: <function_code>,
    args: [ <arg1>, <arg2>, ... ],
    lang: "js"
  }
}
```

#### Açıklamalar:

- `body`: JavaScript fonksiyonu.
- `args`: Fonksiyona gönderilecek argümanlar.
- `lang`: Şu an sadece `"js"` desteklenir.

---

## 🔍 1. Basit Örnek – String Tekrarı

```js
db.test.aggregate([
  {
    $project: {
      original: "$name",
      repeated: {
        $function: {
          body: function(str, count) {
            return str.repeat(count);
          },
          args: ["$name", 3],
          lang: "js"
        }
      }
    }
  }
])
```

> 🔄 `"Ali"` → `"AliAliAli"` olarak tekrar eder.

---

## 🛡️ 2. Güvenli Kullanım — Hata Kontrolüyle

```js
$function: {
  body: function(str, count) {
    if (typeof str !== 'string') return "";
    return str.repeat(count);
  },
  args: ["$name", 3],
  lang: "js"
}
```

> 🛡️ Bu versiyon, `null`, `undefined` veya sayısal veri gelirse hata fırlatmaz.

---

## 🧮 3. Sayı İşlemi — Kare Alma

```js
db.test.aggregate([
  {
    $project: {
      value: 5,
      square: {
        $function: {
          body: function(x) {
            return x * x;
          },
          args: [5],
          lang: "js"
        }
      }
    }
  }
])
```

> 5 → 25

---

## 🧠 4. Koşullu İşlem — Not Hesaplama

```js
db.students.aggregate([
  {
    $project: {
      name: 1,
      note: 1,
      status: {
        $function: {
          body: function(note) {
            return note >= 50 ? "Geçti" : "Kaldı";
          },
          args: ["$note"],
          lang: "js"
        }
      }
    }
  }
])
```

---

## 🔁 5. Tarih Formatlama Örneği

```js
db.logs.aggregate([
  {
    $project: {
      originalDate: "$createdAt",
      formattedDate: {
        $function: {
          body: function(date) {
            return date.getFullYear() + "-" + (date.getMonth()+1).toString().padStart(2, "0") + "-" + date.getDate().toString().padStart(2, "0");
          },
          args: ["$createdAt"],
          lang: "js"
        }
      }
    }
  }
])
```

> `2025-07-15T12:00:00Z` → `"2025-07-15"`

---

## ⚠️ Dikkat Edilmesi Gerekenler

|🛠️ Özellik|Açıklama|
|---|---|
|🧠 Performans|`$function` CPU yoğun bir işlemdir, büyük verilerde yavaş olabilir.|
|🚫 Sharding|`$function`, **sharded** (parçalanmış) sorgularda bazı sınırlamalara sahip olabilir.|
|💡 Yerine Kullanılabilecekler|Basit işlemler için yerleşik `$concat`, `$substrCP`, `$toLower`, `$toUpper` gibi operatörleri tercih et.|

---

## 🧪 Uygulamalı Soru

> TELNR alanındaki numarayı `+90 (xxx) xxx xx xx` formatına dönüştür:

```js
db.lab.aggregate([
  {
    $project: {
      telFormatted: {
        $function: {
          body: function(tel) {
            return "+90 (" + tel.slice(0,3) + ") " + tel.slice(3,6) + " " + tel.slice(6,8) + " " + tel.slice(8,10);
          },
          args: ["$TELNR"],
          lang: "js"
        }
      }
    }
  }
])
```

---

## 🎓 Özet

|Konu|Açıklama|
|---|---|
|`$function`|JavaScript fonksiyonu çalıştırmak için kullanılır|
|En az sürüm|MongoDB 4.4|
|Kullanım alanı|Özel string/sayı/koşul/tarih işlemleri|
|Alternatif|Yerleşik `$` operatörleri varsa onları kullanmak daha performanslıdır|

---

Hazırsan örnek veriyle birlikte ileri seviye özel senaryolara da geçebiliriz. Devam edelim mi?