
> `$exp`, verilen sayının **e (Euler sayısı, yaklaşık 2.71828)** üzeri şeklinde değerini hesaplar.  
> Matematiksel olarak:  
> exe^{x}  
> Burada _x_, verilen sayıdır.

📌 Söz dizimi:

```js
{ $exp: <sayısal ifade> }
```

---

## 📁 Örnek Veri: `values`

```js
db.values.insertMany([
  { x: 0 },
  { x: 1 },
  { x: 2 },
  { x: -1 }
])
```

---

## 📌 1. Üstel değer hesaplama

```js
db.values.aggregate([
  {
    $project: {
      x: 1,
      expValue: { $exp: "$x" }
    }
  }
])
```

📤 Çıktı:

```json
{ x: 0, expValue: 1 }          // e^0 = 1
{ x: 1, expValue: 2.7182818 } // e^1 ≈ 2.71828
{ x: 2, expValue: 7.389056 }  // e^2 ≈ 7.38906
{ x: -1, expValue: 0.367879 } // e^-1 ≈ 0.36788
```

---

## 📌 2. Hesap sonrası yuvarlama

```js
db.values.aggregate([
  {
    $project: {
      x: 1,
      expRounded: {
        $round: [
          { $exp: "$x" },
          3
        ]
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

|Durum|Sonuç / Etki|
|---|---|
|Herhangi sayı|Sorunsuz çalışır|
|Çok büyük x|Sonuç çok büyük olabilir, overflow riski|
|`null`, string|Hata verir|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|e üzeri x hesaplama|✅|
|Negatif ve pozitif x|✅|
|Hesaplama sonrası yuvarlama|✅|
|Aggregation pipeline içinde kullanılabilir|✅|

---

Üstel fonksiyon özellikle:

- Doğal büyüme/azalma modellerinde
    
- Finansal hesaplamalarda (faiz, büyüme oranı)
    
- İstatistik ve makine öğrenmesinde (örneğin log-loss fonksiyonları)
    
- Fizik ve mühendislik hesaplarında
    

çok kullanılır.

---

Devamında hangi operatörle veya konu ile ilerleyelim?