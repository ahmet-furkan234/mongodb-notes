
`$filter`, **bir dizi (array)** içerisinden sadece **belirli koşula uyan elemanları** seçip yeni bir dizi oluşturur.

> Tıpkı JavaScript’teki `array.filter(...)` gibi düşünebilirsin.

---

## 📚 Söz Dizimi

```js
{
  $filter: {
    input: <dizi>,
    as: <takmaAd>,
    cond: <şart ifadesi>
  }
}
```

|Alan|Anlamı|
|---|---|
|`input`|Filtrelemek istediğin dizi|
|`as`|Her eleman için kullanılacak takma isim|
|`cond`|Her eleman için uygulanacak koşul|

🧠 Not: `as` ile tanımlanan elemanlara `$$` ile erişilir: `$$eleman`

---

## ✅ Temel Örnek

### 🎯 Veri:

```json
{
  "_id": 1,
  "notlar": [45, 60, 85, 30, 75]
}
```

### 🎯 Amaç: Sadece 50 ve üzeri notları al

```js
{
  $project: {
    geçerNotlar: {
      $filter: {
        input: "$notlar",
        as: "n",
        cond: { $gte: ["$$n", 50] }
      }
    }
  }
}
```

### 📤 Çıktı:

```json
{
  "_id": 1,
  "geçerNotlar": [60, 85, 75]
}
```

---

## 🧠 `as` nedir?

`as: "n"` dersek, dizideki her elemana `$$n` ismiyle erişiriz.  
Eğer `as` yazmazsan varsayılan `$$this` olur.

---

### ✅ `as` kullanmadan örnek (`$$this`)

```js
{
  $project: {
    büyükler: {
      $filter: {
        input: "$notlar",
        cond: { $gt: ["$$this", 70] } // default: $$this
      }
    }
  }
}
```

---

## 🧪 Orta Düzey Örnek: Obje Dizisi

### 🎯 Veri:

```json
{
  "_id": 1,
  "students": [
    { "name": "Ahmet", "score": 45 },
    { "name": "Mehmet", "score": 80 },
    { "name": "Zeynep", "score": 95 }
  ]
}
```

### 🎯 Amaç: Sadece 50 üzeri öğrencileri al

```js
{
  $project: {
    başarılılar: {
      $filter: {
        input: "$students",
        as: "s",
        cond: { $gte: ["$$s.score", 50] }
      }
    }
  }
}
```

### 📤 Çıktı:

```json
{
  "başarılılar": [
    { "name": "Mehmet", "score": 80 },
    { "name": "Zeynep", "score": 95 }
  ]
}
```

---

## 🧩 `$filter` + `$first` ile eşleşen ilk elemanı almak

> `$filter` her zaman **dizi döner**. Eğer ilk elemanı almak istersen `$first` ile kullan:

```js
{
  $let: {
    vars: {
      eşleşen: {
        $first: {
          $filter: {
            input: "$students",
            as: "s",
            cond: { $eq: ["$$s.name", "Ahmet"] }
          }
        }
      }
    },
    in: "$$eşleşen.score"
  }
}
```

Bu ifade `"Ahmet"`'in notunu getirir.

---

## ❗️ Dikkat Edilecekler

|Durum|Ne olur?|
|---|---|
|Şart sağlanmazsa|Boş dizi döner (`[]`)|
|Girdi `null` ise|`null` döner|
|`as` eksikse|`$$this` varsayılan olarak çalışır|

---

## ✅ Ne Zaman `$filter` Kullanılır?

- Dizideki sadece **bazı elemanları** almak istiyorsan
- Şarta göre **içerikten ayıklama** yapacaksan
- `$map` + `$filter` ile eşleştirme-kıyas yapacaksan
- `$first + $filter` ile eşleşen bir nesneyi bulacaksan
