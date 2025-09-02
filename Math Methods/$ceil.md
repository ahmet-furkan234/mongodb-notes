
- **Tamsayı üst limit belirleme**,
- KDV, indirim, fiyat gibi işlemlerde yukarı yuvarlama,
- Saat, zaman bloklamada fazlalığı garantiye alma,
- Grafikte kategori eşleme gibi yerlerde oldukça faydalıdır.

---

# 🔼 `$ceil` — Yukarı Yuvarlama (Ceiling)

---

## 🎯 Nedir?

> `$ceil`, bir sayıyı **bir üst tamsayıya** yuvarlar.  
> Her zaman yukarı yuvarlama yapar (küçük bile olsa).

📌 Söz dizimi:

```js
{ $ceil: <sayısal ifade> }
```

---

## 📁 Örnek Veri: `products`

```js
db.products.insertMany([
  { name: "TV",     price: 999.01 },
  { name: "Phone",  price: 1250.95 },
  { name: "Laptop", price: 3099.0001 }
])
```

---

## 📌 1. Sayıyı yukarı yuvarla

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      ceilPrice: { $ceil: "$price" }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", ceilPrice: 1000 }
{ name: "Phone", ceilPrice: 1251 }
{ name: "Laptop", ceilPrice: 3100 }
```

---

## 📌 2. Hesap sonrası sonucu yukarı yuvarla

> %8 KDV ekle ve sonucu **yukarı** yuvarla

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      priceWithKDV: {
        $ceil: {
          $add: [
            "$price",
            { $multiply: ["$price", 0.08] }
          ]
        }
      }
    }
  }
])
```

---

## 📌 3. Saat dilimi/slot hesaplama

> Örneğin bir işlem 45 dakikalık bloklara bölünecekse, kaç blok gerektiğini bul:

```js
db.tasks.aggregate([
  {
    $project: {
      durationMinutes: 73,
      requiredSlots: {
        $ceil: {
          $divide: [73, 45] // → 1.62 → 2
        }
      }
    }
  }
])
```

---

## 📌 4. `$cond` ile: fiyat 1000’in üstündeyse yukarı yuvarla

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      adjustedPrice: {
        $cond: {
          if: { $gt: ["$price", 1000] },
          then: { $ceil: "$price" },
          else: "$price"
        }
      }
    }
  }
])
```

---

## 📌 5. `%` veya oranlı değer yukarıya yuvarla (grafik vb.)

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      priceLevel: {
        $ceil: {
          $divide: ["$price", 1000] // 1000’e göre kaçıncı seviye?
        }
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

|Girdi|Sonuç|
|---|---|
|99.01|100|
|99.99|100|
|-2.1|-2|
|0.0001|1|
|`null`|`null`|
|string/bool|❌ Hata|

---

## 📚 `$ceil` vs Diğer Yuvarlama Operatörleri

|Operatör|Açıklama|99.5|
|---|---|---|
|`$ceil`|Yukarı tamsayıya yuvarlar|100|
|`$floor`|Aşağı tamsayıya yuvarlar|99|
|`$round`|En yakın tamsayıya yuvarlar|100|
|`$trunc`|Ondalık kısmı atar (sıfıra doğru)|99|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|Sayıyı yukarı yuvarlama|✅|
|Hesap sonrası yukarı yuvarlama|✅|
|Slot/blok hesaplama|✅|
|`%`, oran, grafik hesapları|✅|
|`$cond` ile koşullu kombinasyon|✅|

---

Sırada istersen:

- 🔻 `$trunc` – Ondalık kısmı kırpma
    
- ➕ `$abs` – Mutlak değer
    
- 🎯 `$cond` – Koşul
    
- 🗓 `$dateAdd`, `$dateSubtract` – Tarih işlemleri
    
- 📐 `$round` ile kıyaslama örnekleri
    

Hangisiyle devam edelim TkMatE?