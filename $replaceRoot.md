


**`$replaceRoot`**, mevcut belgedeki tüm yapıyı değiştirip yerine **başka bir alanı veya hesaplanan yapıyı** koyar.

> Tıpkı: `document = document.newRoot` gibi düşünebilirsin.

---

## 📚 Söz Dizimi

```js
{
  $replaceRoot: {
    newRoot: <yeniBelge>
  }
}
```

---

## ✅ Temel Örnek

### 🎯 Veri:

```json
{
  "_id": 1,
  "user": {
    "name": "Ahmet",
    "age": 30
  }
}
```

### 🎯 Amaç: `user` alanını root belge haline getirmek

```js
{
  $replaceRoot: {
    newRoot: "$user"
  }
}
```

### 📤 Çıktı:

```json
{
  "name": "Ahmet",
  "age": 30
}
```

📌 `_id` bile artık görünmez, çünkü tamamen root değiştirildi.

---

## 💡 Neden Kullanılır?

|Durum|Kullanım Nedeni|
|---|---|
|`lookup` sonrası iç içe belge varsa|İçeriği root yapıp sadeleştirmek|
|`$facet`, `$map`, `$let` gibi işlemler sonrası|Sonuçları doğrudan root yapmak için|
|Dizi içinden tek eleman çıkarıldıysa (`$unwind`)|Onu doğrudan belge haline getirmek|

---

## 🧪 Orta Seviye Örnek

### 🎯 Veri:

```json
{
  "_id": 1,
  "orderId": "A123",
  "customer": {
    "name": "Zeynep",
    "city": "İzmir"
  }
}
```

```js
{
  $replaceRoot: {
    newRoot: {
      name: "$customer.name",
      sehir: "$customer.city",
      siparis: "$orderId"
    }
  }
}
```

### 📤 Çıktı:

```json
{
  "name": "Zeynep",
  "sehir": "İzmir",
  "siparis": "A123"
}
```

---

## 🔁 `$unwind` + `$replaceRoot` Kullanımı

### 🎯 Veri:

```json
{
  "_id": 1,
  "products": [
    { "name": "Kalem", "price": 10 },
    { "name": "Defter", "price": 20 }
  ]
}
```

```js
[
  { $unwind: "$products" },
  { $replaceRoot: { newRoot: "$products" } }
]
```

### 📤 Çıktı:

```json
{ "name": "Kalem", "price": 10 }
{ "name": "Defter", "price": 20 }
```

> Her ürün ayrı belge oldu ve tüm dış yapılar atıldı (örneğin `_id` bile gitti).

---

## 🎯 `$map` sonrası `$replaceRoot` Kullanımı

```js
[
  {
    $project: {
      liste: {
        $map: {
          input: [1, 2],
          as: "x",
          in: { sayı: "$$x", kare: { $multiply: ["$$x", "$$x"] } }
        }
      }
    }
  },
  { $unwind: "$liste" },
  { $replaceRoot: { newRoot: "$liste" } }
]
```

### 📤 Çıktı:

```json
{ "sayı": 1, "kare": 1 }
{ "sayı": 2, "kare": 4 }
```

---

## 🧱 `$replaceRoot` vs `$project`

|Özellik|`$replaceRoot`|`$project`|
|---|---|---|
|Root'u tamamen değiştirir|✅|❌ sadece alanları seçer|
|Yeni belge yapısı sağlar|✅|✅ ama içinde eski root kalır|
|`_id` vs diğer alanlar|Tümü silinir (istersen eklersin)|`_id` kalır (istersen gizlersin)|

---

## 🧠 Kısaca Akılda Kalması İçin:

```js
$replaceRoot: {
  newRoot: "$<alan>"  // örnek: "$user", "$liste", "$product"
}
```

📌 Genelde:

- `$map` sonrası → `$unwind` → `$replaceRoot`
    
- `$lookup` sonrası → iç alan root’a taşınır
    

---

## Devam Seçeneği

Hazırsan şimdi uygulamalı bir örnekle:

- `$facet` ile gelen iki dizi sonucu
    
- `$map` ile eşleştirip karşılaştırma
    
- `$replaceRoot` ile sade gösterim
    

gibi gerçek bir senaryo üzerine geçebiliriz.

Yoksa bu 4 konunun (`$map`, `$let`, `$filter`, `$replaceRoot`) **birleştirildiği** mini quiz veya örnek yapabiliriz.

Ne yapalım TkMatE? 😊