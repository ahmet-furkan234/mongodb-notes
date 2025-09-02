
```js
{
  $switch: {
    branches: [
      { case: <koşul1>, then: <değer1> },
      { case: <koşul2>, then: <değer2> },
      ...
    ],
    default: <hiçbiri uymuyorsa>
  }
}
```

---

## ✅ Gerçek Hayat Örnekleri

---

### 🧪 Örnek 1: Yaşa göre grup (Çocuk/Yetişkin/Yaşlı)

```js
db.getCollection("people").aggregate([
  {
    $project: {
      name: 1,
      age: 1,
      ageGroup: {
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
])
```

---

### 🧪 Örnek 2: Not aralığına göre harf notu

```js
db.getCollection("students").aggregate([
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
])
```

---

### 🧪 Örnek 3: Kullanıcının kuşağını belirle (Y, Z, X vs)

```js
db.getCollection("users").aggregate([
  {
    $project: {
      name: 1,
      birthYear: { $year: { $toDate: "$BIRTHDATE" } },
      generation: {
        $switch: {
          branches: [
            { case: { $gte: [{ $year: { $toDate: "$BIRTHDATE" } }, 2000] }, then: "Z Kuşağı" },
            { case: { $gte: [{ $year: { $toDate: "$BIRTHDATE" } }, 1980] }, then: "Y Kuşağı" },
            { case: { $gte: [{ $year: { $toDate: "$BIRTHDATE" } }, 1965] }, then: "X Kuşağı" }
          ],
          default: "Baby Boomer"
        }
      }
    }
  }
])
```

---

### 🧪 Örnek 4: Sipariş tutarına göre müşteri sınıfı

```js
db.getCollection("orders").aggregate([
  {
    $project: {
      customer: 1,
      total: 1,
      customerLevel: {
        $switch: {
          branches: [
            { case: { $gte: ["$total", 1000] }, then: "Altın" },
            { case: { $gte: ["$total", 500] }, then: "Gümüş" },
            { case: { $gte: ["$total", 250] }, then: "Bronz" }
          ],
          default: "Normal"
        }
      }
    }
  }
])
```

---

## ⚖️ `$switch` vs `$cond` Karşılaştırması

|Özellik|`$cond`|`$switch`|
|---|---|---|
|Tek koşul|✅|✅|
|Birden fazla koşul|❌ karmaşıklaşır|✅ çok uygun|
|`else-if` yapısı|❌ yok|✅ var|
|Okunabilirlik|Orta|Yüksek|

---

## 🔧 Nerelerde Kullanılır?

- Yaşa göre kuşak belirleme
- Not aralığı → Harf notu
- Fiyat/puan aralığı → Kategori sınıflandırma
- Kullanıcının aktivitesine göre rozet belirleme
- Ürün durumuna göre etiket (stokta, tükendi, indirime girdi vs.)

---

