
## 🔧 Değişkenlere Erişim Kuralları

|Tür|Erişim Şekli|Örnek|
|---|---|---|
|Normal alan|`$alanAdı`|`$price`, `$name`, `$city`|
|`$let` içindeki değişken|`$$değişkenAdı`|`$$fiyat`, `$$ortalama`|

> 🧠 Not: `$$` = "Bu bir değişken, alan değil" demek.

---

## ✅ Örnekle Açıklayalım

### 🎯 Veri:

```json
{
  "_id": 1,
  "name": "Ahmet",
  "price": 100,
  "tax": 0.18
}
```

---

### ❌ Yanlış Kullanım:

```js
{
  $project: {
    kdvli: {
      $let: {
        vars: {
          kdvliFiyat: { $multiply: ["$price", { $add: [1, "$tax"] }] }
        },
        in: "$kdvliFiyat"  // ❌ HATA: $ ile erişmeye çalışıyor
      }
    }
  }
}
```

📛 Bu ifade **hata verir**, çünkü `$kdvliFiyat` bir **alan adı gibi algılanır**.

---

### ✅ Doğru Kullanım:

```js
{
  $project: {
    kdvli: {
      $let: {
        vars: {
          kdvliFiyat: { $multiply: ["$price", { $add: [1, "$tax"] }] }
        },
        in: "$$kdvliFiyat"  // ✅ Değişkene doğru erişim
      }
    }
  }
}
```

📤 **Çıktı:**

```json
{
  "_id": 1,
  "kdvli": 118
}
```

---

## 🧠 Aklında Kalması İçin:

```js
// alan = $alanAdı
// değişken = $$değişkenAdı
```

---

## 🎯 Gerçekçi Örnek: Ortalama ve Durum

```js
{
  $project: {
    ad: "$name",
    ortalama: {
      $let: {
        vars: {
          ort: { $avg: ["$midterm", "$final"] }
        },
        in: "$$ort"  // ✅ doğru erişim
      }
    },
    durum: {
      $let: {
        vars: {
          ort: { $avg: ["$midterm", "$final"] }
        },
        in: {
          $cond: [
            { $gte: ["$$ort", 50] },
            "Geçti",
            "Kaldı"
          ]
        }
      }
    }
  }
}
```

---

## 🎓 Ekstra Bilgi

Eğer `$map`, `$filter` gibi operatörlerin **`as` parametresiyle oluşturduğu değişkenler varsa**, bunlara da `$$` ile erişirsin:

```js
{
  $map: {
    input: "$students",
    as: "s",
    in: "$$s.name"  // ✅ map içindeki eleman
  }
}
```

---

## ✅ Kısaca Özet

|Nereden geliyor?|Nasıl erişilir?|Not|
|---|---|---|
|Belgeden (doküman)|`$alanAdı`|Normal alan|
|`$let` içinden|`$$değişkenAdı`|Geçici değişken|
|`$map`, `$filter` içinden|`$$takmaAd`|Döngü elemanı|
