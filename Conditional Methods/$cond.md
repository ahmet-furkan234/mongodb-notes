
```js
$cond: {
  if: <koşul>,
  then: <doğruysa değer>,
  else: <yanlışsa değer>
}
```

Kısa hali:

```js
$cond: [ <koşul>, <doğruysa>, <yanlışsa> ]
```

Her iki yazım da geçerlidir.

---

## ✅ Temel Kullanım Örnekleri

### 🎯 Örnek 1: Notlara göre Geçti mi?

```js
db.getCollection("students").aggregate([
  {
    $project: {
      name: 1,
      score: 1,
      status: {
        $cond: {
          if: { $gte: ["$score", 50] },
          then: "Geçti",
          else: "Kaldı"
        }
      }
    }
  }
])
```

---

### 📍 Örnek 2: Şehir kontrolü

```js
db.getCollection("users").aggregate([
  {
    $project: {
      name: 1,
      cityLabel: {
        $cond: {
          if: { $eq: ["$CITY", "Ankara"] },
          then: "Başkent",
          else: "Diğer"
        }
      }
    }
  }
])
```

---

### 💸 Örnek 3: Fiyat eşiğine göre indirim uygulama

```js
db.getCollection("products").aggregate([
  {
    $project: {
      name: 1,
      price: 1,
      finalPrice: {
        $cond: {
          if: { $gt: ["$price", 100] },
          then: { $multiply: ["$price", 0.9] },  // %10 indirim
          else: "$price"
        }
      }
    }
  }
])
```

---

### 📆 Örnek 4: Geçmiş mi? Gelecek mi?

```js
db.getCollection("events").aggregate([
  {
    $project: {
      title: 1,
      status: {
        $cond: {
          if: { $lt: [{ $toDate: "$eventDate" }, new Date()] },
          then: "Geçmiş",
          else: "Yaklaşan"
        }
      }
    }
  }
])
```

---

### 🧠 Örnek 5: Cinsiyet bazlı hitap

```js
db.getCollection("users").aggregate([
  {
    $project: {
      name: 1,
      GENDER: 1,
      greeting: {
        $cond: {
          if: { $eq: ["$GENDER", "Male"] },
          then: "Bay",
          else: "Bayan"
        }
      }
    }
  }
])
```

---

## ⛓️ Kısa Format Kullanımı (`[koşul, doğru, yanlış]`)

### 🧪 Aynı örnek kısa formatla:

```js
{
  $cond: [
    { $gte: ["$score", 50] },
    "Geçti",
    "Kaldı"
  ]
}
```

---

## 📦 `$cond` Nerelerde Kullanılır?

|Kullanım Alanı|Açıklama|
|---|---|
|Sınıflandırma|Yaş, skor, tarih vb. değerleri sınıflandırma|
|Etiketleme|Cinsiyet, şehir, grup bazlı etiketler|
|İndirim ve fiyat hesaplama|Belirli eşik değerlerinde işlem yapmak|
|Tarih karşılaştırma|Etkinliğin durumu, geçmiş/gelecek kontrolü|

---

## İsteğe Bağlı: Senaryo Önerisi

Aşağıdaki gibi bir senaryoyu birlikte kodlayabiliriz:

**Senaryo:**

- Kullanıcının doğum yılına göre kuşak belirle (`2000 ve sonrası → Z`, `1980–1999 → Y`, `1965–1979 → X`, diğerleri → Baby Boomer)
    

Yapalım mı? Ya da kendi verine özel `$cond` örneği istersen bana veri yapını söyle yeter.