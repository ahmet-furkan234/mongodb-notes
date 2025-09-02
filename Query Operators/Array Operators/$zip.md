
> **Tanım:**  
> `$zip`, birden fazla diziyi alır ve her bir dizinin **aynı index’teki elemanlarını eşleştirerek** birleştirir.  
> Sonuç: **dizilerden diziler** oluşur → `[[a1, b1], [a2, b2], ...]`

---

### 📌 Söz Dizimi:

```js
{
  $zip: {
    inputs: [ <dizi1>, <dizi2>, ... ],
    useLongestLength: <bool>,       // Opsiyonel
    defaults: [ <varsayılan1>, <varsayılan2>, ... ] // Opsiyonel
  }
}
```

---

## ✅ Basit Örnek

### 🧪 Örnek 1: Ad + Soyad dizilerini eşleştir

```js
{
  $project: {
    fullPairs: {
      $zip: {
        inputs: [["Ali", "Ayşe"], ["Yılmaz", "Demir"]]
      }
    }
  }
}
```

📤 **Çıktı:**

```js
[ ["Ali", "Yılmaz"], ["Ayşe", "Demir"] ]
```

---

## 🧪 Gerçek Senaryo – etrade_rdms’e Uygun

### Örnek 2: Ürün adı ve fiyatı listelerini eşleştir

```js
db.getCollection("PRODUCTS").aggregate([
  {
    $project: {
      name_price_pairs: {
        $zip: {
          inputs: ["$PRODUCT_NAMES", "$PRICES"]
        }
      }
    }
  }
])
```

> Böylece: `["Elma", "Armut"]` + `[10, 15]` → `[[Elma, 10], [Armut, 15]]`

---

## 🧰 `useLongestLength` ve `defaults` Kullanımı

### 🧪 Örnek 3: Farklı uzunluktaki dizileri eşleştir

```js
{
  $project: {
    result: {
      $zip: {
        inputs: [["Ali", "Veli"], ["10"]],
        useLongestLength: true,
        defaults: ["?", 0]
      }
    }
  }
}
```

📤 **Çıktı:**

```js
[ ["Ali", "10"], ["Veli", 0] ]
```

> ❗ Bu sayede eksik elemanlar için **varsayılan** değer atanır.

---

## 🔄 Uygulama Fikri: `$map` ile birleşik kullanım

### Örnek 4: Ürün adı ve fiyatlarını eşleştirip tek string’e çevir

```js
{
  $project: {
    namePriceText: {
      $map: {
        input: {
          $zip: {
            inputs: ["$PRODUCT_NAMES", "$PRICES"]
          }
        },
        as: "pair",
        in: {
          $concat: ["$$pair.0", " - ", { $toString: "$$pair.1" }]
        }
      }
    }
  }
}
```

📤 Çıktı örneği:

```js
[ "Elma - 10", "Armut - 15" ]
```

---

## 🔍 `$zip` Ne Zaman Kullanılır?

|Kullanım Durumu|Açıklama|
|---|---|
|İsim + Değer eşleştirme|Ürün adı + fiyat|
|Ad + Soyad eşleştirme|Kullanıcı adı bilgileri|
|Soru + Cevap eşleştirme|Test sonuçları|
|Label + Value eşleştirme|Raporlama, form kontrolü|
|Dizi verilerini formatlamak|JSON mapping / tabloya dökme|

---

## 🎯 Şimdi Ne Yapalım?

İstersen:

- `PRODUCTS` tablosunda isim + fiyat eşleştirelim
    
- `ORDERDETAILS` tablosunda ürün adı + quantity eşleştirelim
    
- Elde edilen `zip` çıktısını `$map` ile stringleştirelim
    

Hangisini uygulayalım TkMatE?