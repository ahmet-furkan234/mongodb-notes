
> `$abs`, verilen sayının **mutlak değerini** döner.  
> Yani negatif bir sayıysa **pozitife çevirir**, pozitifse olduğu gibi bırakır.

📌 Söz dizimi:

```js
{ $abs: <sayı veya ifade> }
```

---

## 📁 Örnek Veri: `transactions`

```js
db.transactions.insertMany([
  { user: "Ali", amount: -500 },
  { user: "Ayşe", amount: 250 },
  { user: "Veli", amount: -1250 },
  { user: "Zeynep", amount: 1000 }
])
```

---

## 📌 1. Tüm işlemleri mutlak hale getirmek

```js
db.transactions.aggregate([
  {
    $project: {
      user: 1,
      absAmount: { $abs: "$amount" }
    }
  }
])
```

📤 Çıktı:

```json
{ user: "Ali", absAmount: 500 }
{ user: "Ayşe", absAmount: 250 }
{ user: "Veli", absAmount: 1250 }
{ user: "Zeynep", absAmount: 1000 }
```

---

## 📌 2. Kazanç mı, zarar mı? Koşullu gösterim + mutlak değer

```js
db.transactions.aggregate([
  {
    $project: {
      user: 1,
      type: {
        $cond: { if: { $lt: ["$amount", 0] }, then: "Zarar", else: "Kazanç" }
      },
      miktar: { $abs: "$amount" }
    }
  }
])
```

---

## 📌 3. Mutlak fark: `verilen - alınan` → negatif de olabilir → mutlak fark için `$abs`

```js
db.compare.insertOne({ verilen: 1200, alinan: 1500 })

db.compare.aggregate([
  {
    $project: {
      fark: { $subtract: ["$verilen", "$alinan"] },
      mutlakFark: {
        $abs: { $subtract: ["$verilen", "$alinan"] }
      }
    }
  }
])
```

📤 Çıktı:

```json
{ fark: -300, mutlakFark: 300 }
```

---

## 📌 4. Ortalama sapma gibi hesaplar için

```js
db.numbers.insertMany([
  { value: 10 },
  { value: 15 },
  { value: 8 },
  { value: 20 }
])
```

```js
// Ortalama: 13.25 → sapmaları göster
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      sapma: {
        $abs: { $subtract: ["$value", 13.25] }
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

|Girdi|Sonuç|
|---|---|
|10|10|
|-5|5|
|0|0|
|null|null|
|string|❌ Hata|

---

## ✅ Özet

|Özellik|Destekleniyor|
|---|---|
|Negatif sayıyı pozitife çevirme|✅|
|Pozitif sayıya dokunmama|✅|
|Matematiksel işlem kombinasyonu|✅|
|`$cond`, `$subtract` ile kullanım|✅|

---

### 🔧 Diğer Faydalı Matematiksel Operatörler

|Operatör|Açıklama|
|---|---|
|`$abs`|Mutlak değer|
|`$trunc`|Ondalık kısmı kırpar|
|`$floor`|Aşağı yuvarlar|
|`$ceil`|Yukarı yuvarlar|
|`$round`|En yakın değere yuvarlar|
