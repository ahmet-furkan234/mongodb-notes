
### 1. **$cond** → if/else gibi düşün

```js
$cond: {
  if: <koşul>,
  then: <koşul doğruysa>,
  else: <koşul yanlışsa>
}
```

---

### ✅ Örnek 1: Geçti/Kaldı Not Analizi

```js
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
```

---

### ✅ Örnek 2: İki şehirden hangisinde yaşıyor?

```js
{
  $project: {
    name: 1,
    cityGroup: {
      $cond: {
        if: { $eq: ["$CITY", "İstanbul"] },
        then: "Marmara",
        else: "Diğer"
      }
    }
  }
}
```

---

## 🔀 2. **$switch** → Çoklu if-else zinciri gibi

```js
{
  $switch: {
    branches: [
      { case: <koşul1>, then: <değer1> },
      { case: <koşul2>, then: <değer2> },
      ...
    ],
    default: <hiçbiri olmazsa>
  }
}
```

---

### ✅ Örnek 3: Yaş grubuna ayırma

```js
{
  $project: {
    name: 1,
    age: 1,
    group: {
      $switch: {
        branches: [
          { case: { $lt: ["$age", 18] }, then: "Çocuk" },
          { case: { $lt: ["$age", 65] }, then: "Yetişkin" }
        ],
        default: "Yaşlı"
      }
    }
  }
}
```

---

### ✅ Örnek 4: Not harf dönüşümü

```js
{
  $project: {
    name: 1,
    score: 1,
    grade: {
      $switch: {
        branches: [
          { case: { $gte: ["$score", 90] }, then: "A" },
          { case: { $gte: ["$score", 80] }, then: "B" },
          { case: { $gte: ["$score", 70] }, then: "C" },
          { case: { $gte: ["$score", 60] }, then: "D" }
        ],
        default: "F"
      }
    }
  }
}
```

---

### ✅ Örnek 5: Cinsiyete göre etiketleme

```js
{
  $project: {
    name: 1,
    gender: 1,
    title: {
      $switch: {
        branches: [
          { case: { $eq: ["$gender", "Male"] }, then: "Bay" },
          { case: { $eq: ["$gender", "Female"] }, then: "Bayan" }
        ],
        default: "Belirsiz"
      }
    }
  }
}
```

---

## 💡 Nerelerde Kullanılır?

|Senaryo|Kullanım|
|---|---|
|Not durumu|`$cond`, `$switch`|
|Yaş sınıflandırması|`$switch`|
|Bölge sınıflandırma|`$cond`|
|Cinsiyet, medeni hal gibi etiketler|`$switch`|
|Tarih kontrolü (geçmiş/gelecek)|`$cond` + `$gte`, `$lt`|

---

## 🧪 Hazır mısın?

Sana özel bir örnek hazırlamamı ister misin?  
Mesela:

- Kullanıcıların doğum yılına göre kuşak grubu ("Z", "Y", "X" gibi)
    
- Sipariş tutarına göre "Bronz", "Gümüş", "Altın" müşteri gibi
    

İstediğini yaz, hemen birlikte geliştirelim.