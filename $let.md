
`$let`, bir veya daha fazla **geçici değişken tanımlayıp**, bu değişkenleri `in` içinde kullanmanı sağlar.

> 📌 Tıpkı programlama dillerindeki `let`, `var`, `const` gibi ama sadece **bir ifade bloğu içinde geçerli**.

---

## 📚 Söz Dizimi

```js
{
  $let: {
    vars: {
      <değişkenAdı>: <değer>,
      ...
    },
    in: <kullanım ifadesi>
  }
}
```

|Alan|Açıklama|
|---|---|
|`vars`|Geçici değişkenleri tanımladığın alan|
|`in`|Bu değişkenlerle ne yapacağını belirttiğin alan|

---

## ✅ Temel Örnek

**Veri:**

```json
{ "_id": 1, "price": 100, "taxRate": 0.18 }
```

**Amaç:** KDV’li fiyatı hesapla ve onu yuvarla

```js
{
  $project: {
    kdvliFiyat: {
      $let: {
        vars: {
          hesapliFiyat: { $multiply: ["$price", { $add: [1, "$taxRate"] }] }
        },
        in: { $round: ["$$hesapliFiyat", 2] }
      }
    }
  }
}
```

📤 Çıktı:

```json
{
  "_id": 1,
  "kdvliFiyat": 118
}
```

---

## 🧪 Orta Düzey Örnek – Ortalama ve Durum Hesaplama

```json
{
  "_id": 1,
  "name": "Ahmet",
  "midterm": 60,
  "final": 80
}
```

```js
{
  $project: {
    ad: "$name",
    ortalama: {
      $let: {
        vars: {
          ort: { $avg: ["$midterm", "$final"] }
        },
        in: "$$ort"
      }
    },
    durum: {
      $let: {
        vars: {
          ort: { $avg: ["$midterm", "$final"] }
        },
        in: {
          $cond: [{ $gte: ["$$ort", 50] }, "Geçti", "Kaldı"]
        }
      }
    }
  }
}
```

📌 Yukarıda `ort` değişkenini hem `ortalama` hem `durum` içinde kullanıyoruz.

---

## ✅ `$let` ile `$map` İçinde Kullanım

Özellikle `$map` içinde `$let` çok işe yarar:

```js
$map: {
  input: "$students",
  as: "s",
  in: {
    $let: {
      vars: {
        avg: { $avg: ["$$s.midterm", "$$s.final"] }
      },
      in: {
        ad: "$$s.name",
        notOrt: "$$avg",
        geçtimi: { $cond: [{ $gte: ["$$avg", 50] }, "Evet", "Hayır"] }
      }
    }
  }
}
```

---

## 📌 Neden `$let` Kullanmalı?

|Durum|Sebep|
|---|---|
|Aynı ifadeyi birden fazla yerde kullanacaksan|Tek sefer yaz, tekrar etme|
|Karmaşık formülleri sadeleştirmek istiyorsan|Ad koyarak anlamlılaştır|
|Performansı artırmak istiyorsan|Hesapları tekrar yapmaz|

---

## 🧠 Kural:

- `$let` içindeki değişkenlere `$$` ile erişirsin.
- `$$değişkenAdı` → bu `vars` içinde tanımlanmış bir şey olmalı.
- Diğer alanlara ise `$alanAdı` diye erişirsin.

---

## 🎯 Gerçek Kullanım: `prevMonthSale` içinde şehir eşleştirme

```js
öncekiAykiSatış: {
  $let: {
    vars: {
      matched: {
        $first: {
          $filter: {
            input: "$prevMonthSale",
            as: "prev",
            cond: { $eq: ["$$prev._id", "$$thisMonth._id"] }
          }
        }
      }
    },
    in: "$$matched.totalSale"
  }
}
```

➡️ `$filter` ile şehir eşleştirilir → `$let` içinde saklanır → sonra sade şekilde kullanılır.

---

## 🎁 Kapanış

|Özellik|Anlamı|
|---|---|
|`vars`|Geçici hesaplamaları buraya koyarsın|
|`in`|Değişkenleri kullanacağın ifade|
|`$$`|`$let` içindeki değişkenlere erişim|
