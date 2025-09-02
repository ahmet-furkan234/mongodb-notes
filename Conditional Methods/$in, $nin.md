
### 🔹 `$in` → Değer, belirtilen **liste içinde** mi?

### 🔹 `$nin` → Değer, **liste içinde değil mi?**

---

## 📌 Söz Dizimleri

### ✅ `$in`

```js
{ alan: { $in: [değer1, değer2, ...] } }
```

### ✅ `$nin`

```js
{ alan: { $nin: [değer1, değer2, ...] } }
```

---

## 🧪 Basit Örnekler

### 1. `CITY` alanı "Ankara", "İzmir" veya "İstanbul" olanları getir:

```js
db.getCollection("USERS").find({
  CITY: { $in: ["Ankara", "İzmir", "İstanbul"] }
})
```

---

### 2. `CATEGORY_ID` 1, 3 veya 5 **olmayan** ürünleri getir:

```js
db.getCollection("PRODUCTS").find({
  CATEGORY_ID: { $nin: [1, 3, 5] }
})
```

---

## 🎯 Aggregation İçinde Kullanım

### 3. `$project` + `$cond` + `$in` kullanımı

```js
db.getCollection("USERS").aggregate([
  {
    $project: {
      name: 1,
      sehirGrubu: {
        $cond: {
          if: { $in: ["$CITY", ["Ankara", "İzmir", "İstanbul"]] },
          then: "Büyükşehir",
          else: "Diğer"
        }
      }
    }
  }
])
```

---

## 🚨 `$in` ile Dizi Üzerinde Arama (Elemanlar için)

```js
{
  $match: {
    INTERESTS: { $in: ["yüzme"] }
  }
}
```

> Bu, `INTERESTS` dizisi `"yüzme"` içeriyor mu diye kontrol eder.

---

## 🔁 `$in` & `$nin` Nerede Kullanılır?

|Kullanım|Açıklama|
|---|---|
|`$match` içinde|Belirli şehir/kategori/ürün filtreleme|
|`$cond` içinde|Bir değerin gruba ait olup olmadığını kontrol etme|
|`$project` ile birlikte|Etiket, sınıflandırma|
|Dizilerle birlikte|Dizinin belirli bir değeri içerip içermediğini kontrol etme|

---

## 💡 Kıyaslama: `$eq` vs `$in`

|Operatör|Ne işe yarar|
|---|---|
|`$eq`|Tek bir değeri kontrol eder|
|`$in`|Birden fazla değerle karşılaştırır|
|`$nin`|Bir değerin dahil **olmadığını** kontrol eder|

---

## 🔬 Bonus: `$in` + `$switch`

```js
{
  $switch: {
    branches: [
      {
        case: { $in: ["$CITY", ["Ankara", "İzmir"]] },
        then: "Ege/İç Anadolu"
      },
      {
        case: { $in: ["$CITY", ["İstanbul", "Bursa"]] },
        then: "Marmara"
      }
    ],
    default: "Diğer"
  }
}
```

---

## ✅ İstersen Uygulamalı Örnek?

Eğer hazırsan, senin `etrade_rdms` veritabanına uygun şekilde örnek yazayım:

**Soru:**  
Sipariş adresinin şehri `"İstanbul"`, `"Ankara"` veya `"İzmir"` olanlara `"Büyükşehir"` etiketi ver.

Bunu ister `$project` içinde, ister `$match` içinde birlikte yazabiliriz.  
Hangi tabloda yapalım istersin? `ADDRESS`, `ORDERS` veya birleştirilmiş?