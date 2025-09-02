
Bu operatör genellikle:
- Sayıyı en yakın **küçük tam sayıya yuvarlamak**
- Tamsayı aralıklarına bölmek
- Saat, gün, blok gibi zamanlamalarda kırpma yapmak

gibi işlemlerde oldukça işe yarar.

---

> `$floor`, verilen sayıyı **aşağıya yuvarlar**.  
> Ondalık kısmı atarak en yakın küçük **tamsayıyı** döner.

📌 Söz dizimi:

```js
{ $floor: <sayısal ifade> }
```

---

## 📁 Örnek Veri: `products`

```js
db.products.insertMany([
  { name: "TV",     price: 999.79 },
  { name: "Phone",  price: 1250.14 },
  { name: "Laptop", price: 3099.99 }
])
```

---

## 📌 1. Sayıyı aşağı yuvarlama

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      floorPrice: { $floor: "$price" }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "TV", floorPrice: 999 }
{ name: "Phone", floorPrice: 1250 }
{ name: "Laptop", floorPrice: 3099 }
```

---

## 📌 2. Hesap sonrası yuvarlama

> %10 indirim uygulanmış fiyatı aşağıya yuvarla

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      discounted: {
        $floor: {
          $multiply: ["$price", 0.9]
        }
      }
    }
  }
])
```

---

## 📌 3. Saatlik blok hesaplama (örnek: 24 saati 3 saatlik bloklara böl)

```js
db.logs.aggregate([
  {
    $project: {
      hour: { $hour: "$createdAt" },
      blockIndex: {
        $floor: {
          $divide: [ { $hour: "$createdAt" }, 3 ]
        }
      }
    }
  }
])
```

> Örneğin saat 07:00 → `2. blok` olur (çünkü 7 / 3 = 2.3 → floor = 2)

---

## 📌 4. `$cond` ile: eğer fiyat yüksekse floor uygula

```js
db.products.aggregate([
  {
    $project: {
      name: 1,
      safePrice: {
        $cond: {
          if: { $gt: ["$price", 1000] },
          then: { $floor: "$price" },
          else: "$price"
        }
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

|Durum|Sonuç|
|---|---|
|123.99|123|
|-3.5|-4|
|0.0001|0|
|null|null|
|string, bool|❌ Hata verir|

---

## 📚 Karşılaştırma Tablosu

|Operatör|Açıklama|Örnek (`123.75`)|
|---|---|---|
|`$round`|En yakın değere yuvarlar|`124`|
|`$floor`|Aşağı yuvarlar (küçüğe)|`123`|
|`$ceil`|Yukarı yuvarlar (büyüğe)|`124`|
|`$trunc`|Ondalık kısmı kırpar (0’a doğru)|`123`|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|Sayıyı aşağı yuvarlama|✅|
|Hesaplama sonrası floor|✅|
|Koşullu kullanım (`$cond`)|✅|
|Zaman blokları/indeksleme senaryosu|✅|
|Negatif sayılarda mantıklı sonuç|✅|

---

Hazırsan sırada:

- 🔼 `$ceil` (yukarı yuvarlama)
    
- 🔻 `$trunc` (ondalık kısmı atma)
    
- ➕ `$abs` (mutlak değer)
    
- 🎯 `$cond`, `$switch` (koşul)
    
- 🗓 `$dateAdd`, `$dateSubtract`, `$dateDiff` (tarih işlemleri)
    

hangisiyle devam edelim TkMatE?