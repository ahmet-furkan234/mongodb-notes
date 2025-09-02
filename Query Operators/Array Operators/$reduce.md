
> **Tanım:**  
> `$reduce`, bir dizinin tüm elemanları üzerinde sırayla dolaşarak bir değer üretir (toplama, birleştirme, çarpma vb.).

---

### 📌 Söz Dizimi:

```js
{
  $reduce: {
    input: <dizi>,
    initialValue: <başlangıç değeri>,
    in: <birikimli işlem (accumulator)>
  }
}
```

---

### 🧱 Bileşenler:

|Alan|Açıklama|
|---|---|
|`input`|Üzerinde dönülecek dizi|
|`initialValue`|İlk başlangıç değeri (örneğin `0` veya `""`)|
|`in`|Biriken sonuca her adımda ne yapılacağı (accumulator)|

---

## ✅ Temel Örnekler

### 🧪 Örnek 1: Sayıları toplayalım

```js
{
  $project: {
    toplam: {
      $reduce: {
        input: [10, 20, 30],
        initialValue: 0,
        in: { $add: ["$$value", "$$this"] }
      }
    }
  }
}
```

> `$$value`: o ana kadarki toplam  
> `$$this`: mevcut eleman

---

### 🧪 Örnek 2: Kelimeleri birleştir (cümle yap)

```js
{
  $project: {
    cümle: {
      $reduce: {
        input: ["Ben", "MongoDB", "seviyorum"],
        initialValue: "",
        in: { $concat: ["$$value", " ", "$$this"] }
      }
    }
  }
}
```

---

## 🧪 Gerçek Hayat – etrade_rdms Uygulamalı Örnekler

---

### 📦 Örnek 3: `ORDERDETAILS` içindeki `QUANTITY` dizisini toplayarak toplam adet bul

```js
db.getCollection("ORDERDETAILS").aggregate([
  {
    $project: {
      orderId: 1,
      totalQuantity: {
        $reduce: {
          input: "$QUANTITIES",  // varsayalım bu bir dizi
          initialValue: 0,
          in: { $add: ["$$value", "$$this"] }
        }
      }
    }
  }
])
```

---

### 🛍️ Örnek 4: `PRODUCTS` içinde `TAGS` dizisini virgülle birleştir

```js
db.getCollection("PRODUCTS").aggregate([
  {
    $project: {
      name: 1,
      tagsText: {
        $reduce: {
          input: "$TAGS",
          initialValue: "",
          in: {
            $cond: {
              if: { $eq: ["$$value", ""] },
              then: "$$this",
              else: { $concat: ["$$value", ", ", "$$this"] }
            }
          }
        }
      }
    }
  }
])
```

---

## 🔧 $reduce Kullanım Alanları

|Amaç|Açıklama|
|---|---|
|Toplama|Dizi elemanlarını toplayarak total üretme|
|Birleştirme|String dizilerini tek metin haline getirme|
|Ortalama/Skor|Puan hesaplama|
|Dinamik koşullar|Koşullu işlemleri döngü içinde yapmak|

---

## 🎯 Bonus: $reduce + $map + $filter ile ne yapılır?

```js
// Notları sadece >=50 olanları toplayalım
{
  $project: {
    passedScoreSum: {
      $reduce: {
        input: {
          $filter: {
            input: "$SCORES",
            cond: { $gte: ["$$this", 50] }
          }
        },
        initialValue: 0,
        in: { $add: ["$$value", "$$this"] }
      }
    }
  }
}
```


