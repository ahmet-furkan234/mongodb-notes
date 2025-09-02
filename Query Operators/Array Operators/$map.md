
`$map`, bir dizinin **her elemanı üzerinde dönüp yeni bir dizi oluşturmanı sağlar**.

SQL, JS ya da Python bilenler için:

- JavaScript `array.map()`
- Python `[x * x for x in liste]`
- SQL `SELECT col * 2 FROM ...`

---

## 📚 Söz Dizimi:

```js
{
  $map: {
    input: <dizi>,
    as: <takmaAd>,
    in: <ifade>
  }
}
```

|Alan|Açıklama|
|---|---|
|`input`|Üzerinde dönülecek dizi|
|`as`|Her elemanın takma ismi (varsayılan: `this`)|
|`in`|Her elemandan üretilecek yeni değer|

---

## 🧪 Temel Örnek

```json
{
  "_id": 1,
  "sayilar": [2, 4, 6]
}
```

```js
db.example.aggregate([
  {
    $project: {
      kareler: {
        $map: {
          input: "$sayilar",
          as: "n",
          in: { $multiply: ["$$n", "$$n"] }
        }
      }
    }
  }
])
```

📤 **Çıktı:**

```json
{
  "_id": 1,
  "kareler": [4, 16, 36]
}
```

---

## 🔄 `as` kullanmadan (default `this`)

```js
{
  $map: {
    input: "$sayilar",
    in: { $add: ["$$this", 10] }
  }
}
```

📌 Burada `as` kullanılmadığı için her eleman `$$this` olarak temsil edilir.

---

## 📘 Orta Seviye Örnek – Nesne Dönüşümü

### 🎯 Veri:

```json
{
  "_id": 1,
  "urunler": [
    { "ad": "Kalem", "fiyat": 5 },
    { "ad": "Defter", "fiyat": 10 }
  ]
}
```

### 🎯 Amaç: KDV’li fiyatı hesapla

```js
{
  $project: {
    urunlerKDVli: {
      $map: {
        input: "$urunler",
        as: "u",
        in: {
          ad: "$$u.ad",
          kdvliFiyat: { $multiply: ["$$u.fiyat", 1.2] }
        }
      }
    }
  }
}
```

📤 **Çıktı:**

```json
{
  "urunlerKDVli": [
    { "ad": "Kalem", "kdvliFiyat": 6 },
    { "ad": "Defter", "kdvliFiyat": 12 }
  ]
}
```

---

## 🧠 `$map` içinde `$let` kullanımı (karmaşık dönüşümler)

```js
{
  $map: {
    input: "$students",
    as: "s",
    in: {
      $let: {
        vars: {
          ortalama: { $avg: ["$$s.midterm", "$$s.final"] }
        },
        in: {
          ad: "$$s.name",
          durum: {
            $cond: [{ $gte: ["$$ortalama", 50] }, "Geçti", "Kaldı"]
          }
        }
      }
    }
  }
}
```

---

## 🎯 Gerçekçi Kullanım: `$facet` sonrası eşleştirme

```js
$map: {
  input: "$monthSale",
  as: "thisMonth",
  in: {
    şehir: "$$thisMonth._id",
    buAy: "$$thisMonth.total",
    öncekiAy: {
      $let: {
        vars: {
          eşleşen: {
            $first: {
              $filter: {
                input: "$prevMonthSale",
                as: "prev",
                cond: { $eq: ["$$prev._id", "$$thisMonth._id"] }
              }
            }
          }
        },
        in: "$$eşleşen.total"
      }
    }
  }
}
```

---

## ❗️Uyarılar

|Durum|Açıklama|
|---|---|
|Boş dizi varsa (`[]`)|Boş dizi döner|
|`null` dizi varsa|`null` döner|
|Elemanlarda eksik alan varsa|O alana göre `null` olabilir|

---

## ✅ Ne Zaman `$map` Kullanılır?

- Dizideki her eleman üzerinde dönüp, yeni dizi oluşturacaksan
- Dizi içindeki nesneleri dönüştürmek istiyorsan
- `facet` sonrası veya `$group` sonrası dizi işlemek istiyorsan

